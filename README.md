# MGU Cast 0.3.1 experimental Store release

Native iPhone AirPlay video test for BMW iDrive 7 MGU. No audio. Not a car-validated working video app.

## Install through MGU Store

1. Connect the BMW through MGU Uplink to the same iPhone's Personal Hotspot.
2. Open MGU Store, Sources, Add source, GitHub.
3. Enter `vc7k77yd77-crypto/mgu-cast` and select Releases. The source shorthand is `vc7k77yd77-crypto/mgu-cast:releases`.
4. Refresh the Store and install/update MGU Cast Beta 0.3.1. Its required receiver is `mgu-cast` 0.3.1; the catalog declares this dependency.
5. Install both updates and accept the Store's restart prompt if offered. An older running receiver will report that an update is required until restarted.
6. While parked, open MGU Cast Beta and select Start video test. In iPhone Control Centre, Screen Mirroring, select MGU Cast Test. Enter the PIN shown in the app.
7. Rotate the iPhone to landscape and play a non-protected local video. Each test ends after two minutes; Start begins another session.
8. Stop through the app or leave its video page. Check that casting stops and normal BMW screens remain usable.

If GitHub source entry is unavailable, add this HTTPS catalog source:
`https://github.com/vc7k77yd77-crypto/mgu-cast/releases/download/catalog/catalog.json`

## Behavior and limits

The explicit Start action enables an experimental live decoding path. No vehicle speed, gear or camera subscriptions are requested. Native embed support, video support, foreground ownership, visibility and focus are required; no layer fallback, input grab or BMW UI patch is used. Loss of display ownership stops casting. Actual camera/warning interruption behavior still needs car validation. Nothing starts merely by installing the app.

Only the current hotspot phone and fixed AirPlay ports receive temporary session firewall access. The worker drops root privileges. A separate timed guardian and service stop cleanup remove receiver rules. The root broker listens only on a local socket and authenticates the app UID. No persistent pairing identity is saved. This topology is limited to an iPhone hotspot on 172.20.10.0/28. Other Wi-Fi networks, audio, protected streaming services and general AirPlay media URL playback are unsupported.

## Validation

Local frontend tests cover asynchronous receiver/display readiness, focus and foreground interruption, clearing stale frames between sessions, aspect fit, no vehicle subscriptions and session-only cleanup selection. JavaScript syntax and package contents/checksums passed. The Store's own catalog parser accepts both entries. ELF checks confirm an x86-64 Linux executable targeting glibc 2.27 or earlier.

Earlier car tests established iPhone discovery/PIN pairing/H.264 delivery and separate byte-exact synthetic hardware decoding. Those are separate results; this packaged live display path has not been car-tested. Validate visible playback, landscape rotation, Stop, disconnect, app exit and native camera/warning priority before calling it working.

## Source and build

Download `MGU-Cast-Source-0.3.1.zip` from the catalog release. It includes the public kit, receiver, streaming decoder, dependency sources, service, frontend and tests. No BMW SDK, firmware libraries, keys, vehicle identifiers or pairing logs are included.

Run `node test/gate.js` and `node test/frontend.js` from the extracted source root. For the receiver, use Python 3 and Zig 0.13.0, set MGU_ZIG to its executable, and run `python work/build_airplay.py`. Target: x86_64-linux-gnu.2.27. The generated crypto stub is link-only: never install it. Runtime uses the car's existing libcrypto.so.1.1. Build the peer helper from native/peeruid.c with the same target; app generation is `python kit/tools/build.py mgu_cast`. App SDK generation happens on the car from its local donor.

Public upstream sources: UxPlay 05b0275cb1b726592700e47a6e0307544b3d3a52; mgu-store 54c656a79f08db3385393417a93b5608778fa571; Moonride bbde8edb5b7c347d68a006f07543254b2d783063; libplist 2.6.0; OpenSSL 1.1.1w public headers. Corresponding licenses are included. Project code is GPL-3.0-or-later.
