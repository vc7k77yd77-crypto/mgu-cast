# MGU Cast development beta

Direct iPhone AirPlay video casting for BMW iDrive 7 MGU. This is development source, not a verified working video app. No audio is implemented.

## Current status

Real-car tests confirmed iPhone discovery, PIN pairing and incoming H.264 packets. An independent synthetic H.264 clip decoded on the MGU hardware and matched software decoding byte for byte. The streaming adapter also decoded synthetic input. End-to-end iPhone video display, landscape behavior and native UI preemption still require car testing. The release gate remains disabled until those checks pass.

Version 0.3.0 removes vehicle speed, gear and camera subscriptions and the associated diagnostic file. Rendering requires the app's native embedded display, foreground ownership, visibility and focus. There is no layer fallback or input grab. BMW native display priority is retained by design, but interruption behavior is not yet car-validated.

## Source layout

- `apps/mgu_cast`: minimal Start/Stop frontend and landscape aspect-fit rendering.
- `daemon`: local peer-authenticated receiver supervisor and session firewall cleanup.
- `native`: peer credential helper and Intel Media SDK public ABI headers.
- `receiver`: receiver entry point and streaming decoder adapter.
- `tools/mgu-cast`: MGU Deploy component definition and service.
- `test`: checks runnable on Windows with Node.

Run `node test/gate.js`. Run `node --check` on changed JavaScript files. These checks do not prove playback in the car.

## Build dependencies

The complete downloadable source package includes the public kit and dependency source trees. Build from its root with Python 3 and Zig 0.13.0, targeting x86_64-linux-gnu.2.27. Set MGU_ZIG to the Zig executable, then run `python work/build_airplay.py`. Never deploy the generated link-only crypto stub: the executable resolves the MGU's existing libcrypto.so.1.1. App packaging uses `python kit/tools/build.py mgu_cast`; target-local SDK generation is required and proprietary BMW SDK files are excluded.

Upstream references: [UxPlay](https://github.com/FDH2/UxPlay) 05b0275cb1b726592700e47a6e0307544b3d3a52; [MGU Store](https://github.com/robertnisipeanu/mgu-store) 54c656a79f08db3385393417a93b5608778fa571; [Moonride](https://github.com/JustAnotherPU/moonride) bbde8edb5b7c347d68a006f07543254b2d783063; libplist 2.6.0; OpenSSL 1.1.1w public headers. Preserve their licenses.

## Next car session

Use the experimental build only while parked. Confirm the standard BMW UI works first. Open MGU Cast, start a bounded test, mirror from the hotspot iPhone, enter the displayed PIN and play a non-protected local landscape clip. Verify aspect ratio, frame delivery, Stop, app exit and native camera/warning priority. Disconnect and confirm receiver termination and removal of its session firewall rules. Current tests are limited to two minutes; extending lifetime requires watchdog validation.

## Store availability

A repository is not itself an installable MGU Store release. No validated catalog/release is published yet. Do not add a guessed Store source or expect this source revision to unlock video. Next release needs end-to-end car validation before the video gate is enabled.

No SSH keys, VINs, pairing codes, vehicle SDK files or firmware libraries belong in this repository.
