---
title: "Fixing WireGuard Subnet Conflicts: Why Your VPN Won't Route Traffic"
date: 2025-10-27
draft: false
tags: ["wireguard", "vpn", "networking", "troubleshooting"]
categories: ["Tutorials"]
---

You've spent time setting up WireGuard on your Raspberry Pi, the connection shows as active, you see traffic in the WireGuard interface—but when you try to ping or access anything on your home network, nothing works. **WireGuard is setup but I can't access anything**. This frustrating situation is often caused by a subnet conflict that's invisible at first glance.

## Understanding the Problem

When your VPN won't route traffic despite being "connected," the issue is usually that **your current network and your home network use the same IP subnet**. For example, if both networks use `192.168.1.0/24`, your device doesn't know whether `192.168.1.23` refers to a local device or one back home through the VPN.

## Quick Diagnosis

### Check if the VPN tunnel works

First, connect to the VPN and verify the VPN connection itself is functional:

```bash
# Ping the WireGuard server's VPN IP
ping 10.191.124.1
```

If this works, your VPN tunnel is fine—the problem is routing to your home network.

### Identify the subnet conflict

Check your current network's subnet:

```bash
# On Linux/Mac
ip route | grep default
# or
ifconfig | grep inet

# On Windows
ipconfig
```

If you see `192.168.1.x` and your home network also uses `192.168.1.x`, you've found the cause.

### Confirm with routing table

```bash
# On Mac/Linux
netstat -nr | grep 192.168.1

# On Windows
route print
```

If you see multiple routes for the same subnet pointing to different interfaces, your OS doesn't know which path to take.

## Solution 1: Allow Specific IPs Only (Quick Fix)

The fastest and simplest solution is to **route only specific devices** through the VPN instead of the entire subnet.

### Configuration

Edit your WireGuard client config:

```ini
[Interface]
PrivateKey = YOUR_PRIVATE_KEY
Address = 10.191.124.4/24
DNS = 9.9.9.9

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = YOUR_PUBLIC_IP:51820
# Instead of 192.168.1.0/24, use /32 for specific hosts
AllowedIPs = 192.168.1.1/32, 192.168.1.23/32, 10.191.124.0/24
PersistentKeepalive = 25
```

### What this does

The `/32` notation creates routes for **individual IP addresses only**. These specific routes take priority over your local network's broader subnet route.

### Limitations

- ✅ Works from any network, even if it uses the same subnet
- ✅ No changes needed on the server side
- ❌ You must manually list each device you want to access
- ❌ New devices on your home network won't be automatically accessible
- ❌ Dynamic IPs (DHCP) can break connections if they change (prefer assign IP address)

**Best for:** Accessing a few specific services (Raspberry Pi, NAS, home server)

## Solution 2: Change Your Home Network Subnet (Permanent Fix)

The robust solution is to change your home network to use a different subnet that won't conflict with common networks.

### Recommended subnets

- `10.0.0.0/24` - Unlikely to conflict
- `192.168.2.0/24` - Common alternative
- `172.16.0.0/24` - Rarely used in home networks

### Steps to change

1. **Access your router** (usually `http://192.168.1.1`)
2. **Navigate to LAN/DHCP settings**
3. **Change the IP range** from `192.168.1.x` to `10.0.0.x` (for example)
4. **Save and reboot** the router
5. **Reconnect all devices** (they'll get new IPs automatically)

### Update WireGuard client config

```ini
[Interface]
PrivateKey = YOUR_PRIVATE_KEY
Address = 10.191.124.4/24
DNS = 9.9.9.9

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = YOUR_PUBLIC_IP:51820
# Now you can use the entire subnet without conflicts
AllowedIPs = 10.0.0.0/24, 10.191.124.0/24
PersistentKeepalive = 25
```

### What this does

By using a unique subnet at home, you eliminate any possibility of conflicts. When you connect to the VPN, your device knows that `10.0.0.x` addresses should route through the tunnel because they don't exist on your current local network.

### Benefits and trade-offs

- ✅ Access to **all devices** on your home network automatically
- ✅ Works reliably from any network worldwide
- ✅ New devices are immediately accessible
- ✅ Dynamic DHCP assignments don't break connections
- ⚠️ Requires one-time router reconfiguration
- ⚠️ All devices need to reconnect once (automatic via DHCP)
- ⚠️ Need physical or remote access to your home network initially

**Best for:** Anyone who regularly uses VPN to access their home network

## Why not route everything through the VPN?

You might consider using `AllowedIPs = 0.0.0.0/0` to route all traffic through the VPN, which would solve the subnet conflict. However:

- All your internet traffic (Netflix, YouTube, browsing) goes through your home connection
- Slower performance due to double routing
- Unnecessary load on your home network
- Your home IP address is exposed for all activities

This approach works but is overkill for accessing local resources.

## Server-side considerations

Make sure your WireGuard server has proper forwarding configured. On your Raspberry Pi:

```bash
# Check IP forwarding is enabled
cat /proc/sys/net/ipv4/ip_forward
# Should output: 1

# Verify iptables rules exist
sudo iptables -t nat -L POSTROUTING -v -n
# Should show MASQUERADE rules for your WireGuard subnet
```

If these aren't configured, the VPN tunnel works but traffic won't route to your local network.

## Which solution should you choose?

**Choose Solution 1** (specific IPs) if:
- You only need to access 2-3 specific devices
- You can't change your home network configuration right now
- You need a working solution immediately

**Choose Solution 2** (change subnet) if:
- You regularly use VPN to access multiple home devices
- You want a reliable, permanent solution
- You can spend 10 minutes reconfiguring your router

For most users who seriously use VPN to access their home network, **changing the subnet is worth the one-time effort**. It eliminates the problem permanently and provides the best long-term experience.

## Conclusion

Subnet conflicts are one of the most common but least obvious WireGuard issues. The connection appears to work (traffic flows, handshake succeeds), but routing fails silently. By understanding how your OS handles overlapping routes, you can quickly diagnose and fix the problem with either targeted routes or a subnet change.

**Remember: a working WireGuard connection doesn't guarantee working routes to your destination network. Always verify your routing table matches your intentions.**