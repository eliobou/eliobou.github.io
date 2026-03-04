+++
date = '2026-03-04T17:53:28+01:00'
title = "Rebooting a Firestick Remotely Over the Network"
tags = ["adb", "firestick", "raspberry-pi", "iptv", "docker", "ios-shortcuts"]
draft = false
+++

You're away from home, hotel room, ready to watch a show. You open your streaming app and get a "**maximum streams reached**" error. The Firestick in your living room has been sitting idle all day with a session still open. Nobody's home to unplug it and you are stuck.

This problem can happen to everyone with a subscription (Netflix, IPTV, ...) and is very frustrating. That's how I solved it.

> Repo with full deployment instructions: [github.com/eliobou/network-firestick-rebooter](https://github.com/eliobou/network-firestick-rebooter)


## How It Works

The Firestick runs Fire OS, a custom Amazon Android distribution. When you [enable ADB debugging in the developer options](https://developer.amazon.com/docs/fire-tv/connecting-adb-to-device.html#turnondebugging), it opens port 5555 on your local network and accepts remote commands.

A Raspberry Pi on the same home network acts as the bridge. It runs a minimal Flask server exposing a single `GET /reboot` endpoint. When called, the Pi connects to the Firestick over ADB and triggers a reboot â€” the equivalent of running this locally:

```bash
adb connect 192.168.1.99:5555
adb -s 192.168.1.99:5555 reboot
```

The full chain looks like this:

```
iPhone (iOS Shortcuts) -> VPN -> Raspberry Pi :8765 -> Flask -> ADB -> Firestick
```

One important step that requires physical access: the first time the Pi connects to the Firestick over ADB, a pairing popup appears on the TV screen asking you to authorize the connection. You need to accept it once with the remote and check "Always allow". After that, the Pi is trusted and everything runs headlessly from there.

Since the Pi is only reachable from your local network, you need a VPN or some kind of access to your network to trigger the reboot when you're away. WireGuard on the same Pi works well for this. Once connected, an iOS Shortcut fires a single GET request to the Pi, the Firestick reboots, and your stream slot is freed.

---

Full deployment instructions, Docker setup and configuration in the repo: [github.com/eliobou/network-firestick-rebooter](https://github.com/eliobou/network-firestick-rebooter)