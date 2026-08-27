# Panda RGB Mac Client

A native Swift client for the [BIGTREETECH Panda RGB Controller](https://global.bttwiki.com/Panda_RGB_Controller.html),
built for macOS, iPhone and iPad from a single target.

The controller ships with a web interface. This client exists because a native app can do
things that interface cannot: sit in the menu bar, follow the printer's own chamber light,
and check for firmware releases on its own.

## Status

The repository is set up but the source has not been published here yet. Development
happens in a private repository; this is where the public client will land.

## What it does

- Sets the light effect: static, breathing, strobing, colour cycle, warning, and the H2D
  style pattern.
- Controls brightness and effect speed.
- Edits the palette slots the controller calls blocks.
- Follows the printer's chamber light, so the enclosure lighting tracks the machine
  rather than being set by hand.
- Checks for a newer firmware release, downloads it, and pushes it to the controller.

## Before you run it

The app is not signed with an Apple Developer ID, so macOS will refuse to open it on the first
attempt. **[USAGE.md](USAGE.md) explains what you will see and how to open it**, along with what
can and cannot be verified about the download and what the app is allowed to do.

## Requirements

- macOS 14 or later, or iOS and iPadOS 17 or later.
- A Panda RGB Controller reachable on the local network.
- For following the printer, a Bambu Lab printer in LAN mode, with its serial number and
  access code.

## Licence

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) and
[NOTICE](NOTICE).

You are free to use, modify and redistribute this software, including commercially, at no
cost. The licence grants that permission; it does not transfer ownership. Copyright in
this work remains with Dale Douglas, and the licence requires that this attribution
travels with any copy or derivative. It grants no rights in the project's name or
branding.

Copyright 2026 Dale Douglas.
