# Using the app before it is signed

The app published here is **ad-hoc signed**, not signed with an Apple Developer ID and not
notarised by Apple. macOS treats it accordingly: it will refuse to open it the first time and
say it cannot verify the developer. This page explains what that means, how to open it anyway,
and what you can and cannot check about the download.

## What you will see

Double-clicking the app the first time produces one of these, depending on your macOS version:

- "PandaRGB cannot be opened because Apple cannot check it for malicious software."
- "PandaRGB is damaged and cannot be opened. You should move it to the Bin."

The second message is misleading. The download is not damaged. macOS shows it when a quarantined
app has no recognised signature, and it looks the same as genuine corruption.

## How to open it

Put `PandaRGB.app` in `/Applications`, then either:

**Right-click and choose Open.** Confirm at the prompt. macOS records the exception and normal
double-clicking works from then on. On recent macOS versions the first attempt still fails, and
you then go to System Settings, Privacy & Security, and press **Open Anyway** next to the
message about PandaRGB.

**Or clear the quarantine flag from a terminal:**

```
xattr -d com.apple.quarantine /Applications/PandaRGB.app
```

Either is enough; you do not need both. You should only have to do it once per download.

## What the quarantine flag actually is

Anything downloaded by a browser gets an extended attribute, `com.apple.quarantine`, attached to
it. Gatekeeper checks that attribute on first launch and demands a recognised signature. Removing
the attribute, or granting the exception through the dialog, tells macOS you accept responsibility
for this particular file. It does not disable Gatekeeper generally, and it does not change any
system setting.

To see the flag before you clear it:

```
xattr -l /Applications/PandaRGB.app
```

## What you can verify, and what you cannot

Be clear about the limits. An ad-hoc signature contains no identity, so **it cannot prove the app
came from its author**. That is exactly what an Apple Developer ID would add, and why the step
above exists at all. What you can check is that the bundle is internally consistent and has not
been altered since it was built:

```
codesign --verify --deep --strict /Applications/PandaRGB.app && echo intact
codesign -dv /Applications/PandaRGB.app
```

The first prints `intact` if the bundle has not been altered since it was signed. The second
prints the bundle's details, and the line to look at is the signature:

```
Signature=adhoc
```

`Signature=adhoc` is what this build is expected to report. If you ever see a signature
claiming a named identity, or an identifier that does not end in `.PandaRGB`, the file did not
come from this repository.

If you would rather not accept an unsigned download, build the app yourself from source instead.
The source is not in this repository; ask, and it can be pointed at.

## What the app is allowed to do

It runs in the macOS App Sandbox. Its entitlements are:

| Entitlement | What it permits |
| --- | --- |
| `com.apple.security.app-sandbox` | Runs sandboxed at all. |
| `com.apple.security.network.client` | Makes outbound network connections, which is how it reaches the controller and the printer. |
| `com.apple.security.files.user-selected.read-only` | Reads a file you pick yourself, for choosing a firmware image. |

It cannot accept inbound connections, read your files without you choosing them, or run at login
unless you add it yourself. `com.apple.security.get-task-allow`, which would let a debugger attach
to it, is deliberately absent, and the build refuses to publish if it ever reappears.

## What it needs to be useful

- A Panda RGB Controller reachable on your local network.
- For following the printer's chamber light: a Bambu Lab printer in LAN mode, plus its serial
  number and access code. Those are held in memory only, for the session; they are not written to
  disk.

## Updating

There are no version numbers yet. One release, tagged `latest`, is replaced in place whenever the
app changes, so the download link always gives the current build:

```
https://github.com/DaleDouglasUK/panda_rgb_mac_client/releases/latest/download/PandaRGB-macOS.zip
```

To update, quit the app, replace it in `/Applications`, and clear quarantine again if you used
that route. The release notes record which source commit each build came from, which is the only
way to tell two builds apart until versioning exists.

## When this page goes away

Signing with an Apple Developer ID and notarising removes every step above: the app would open on
a double-click, and its signature would name its author. That needs a paid Apple Developer
account, and until there is one, the instructions here are the way to run it.
