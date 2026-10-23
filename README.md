# companion-satellite-armbian
A build of Companion Satellite that uses Armbian to run on several SBC's
(See also: [companion-armbian](https://github.com/elliotmatson/companion-armbian))

## Downloading an image
Ready-made images for a few Orange Pi boards are attached to each [release](https://github.com/elliotmatson/companion-satellite-armbian/releases). Flash the `.img.xz` for your board to an SD card with balenaEtcher or Raspberry Pi Imager (both handle `.xz` directly). Satellite's web UI is on port 9999, and a subset of its settings can be edited in `/boot/satellite-config` on the device.

SSH is enabled. Log in as `root` with password `1234`; you'll be required to set a new password on first login.

## Using this on your own:
This repo uses Github Actions to build Armbian, and then install Companion Satellite. It then saves the resulting image file as an artifact for you to write to an SD card. To do this process on your own:

 1. Fork this repository.
 2. Navigate to the `Actions` tab on your fork, and select the action `Build armbian + build companion satellite` on the left pane.
 3. Click `Run Workflow`, and select your board and version of Companion Satellite (e.g. `v2.6.0`, or `stable` for the latest release).
 4. Run the workflow
After the workflow completes, navigate to the `Summary` tab of that execution, and your image will be attached as `armbian-<board>-companion-satellite-<version>.zip`, containing an `.img.xz`.

## How it works
The build itself lives in [companion-armbian](https://github.com/elliotmatson/companion-armbian)'s shared `build-image.yaml` workflow: it builds a minimal Ubuntu 26.04 (Resolute) Armbian image for the chosen board from the current [armbian/build](https://github.com/armbian/build) `main` (the exact commit is listed in each release), then uses packer to run Companion Satellite's `pi-image/install.sh` inside it, using `companion-satellite.pkr.hcl` from this repo.

## Automatic releases
Renovate keeps the `companion-satellite` submodule pinned to the latest release tag. Whenever that pin changes on `main`, the `Build Select Boards on Companion Update via Renovate` workflow builds images for a few Orange Pi boards and publishes them as a GitHub release, with Companion Satellite's release notes.
