# RediWM PKGBUILD

Arch Linux packaging recipe for [RediWM](https://github.com/oxydizer/rediwm),
a Wayland compositor and desktop shell written in Zig.

## Build

Install the required Arch Linux build tools and dependencies, then run:

    makepkg -si

To build without installing:

    makepkg -s

The recipe fetches the upstream RediWM Git repository and builds a release-safe
package. Build artifacts and extracted source directories are not tracked here.

## Package notes

- Package: `rediwm-git`
- Architecture: `x86_64`
- Provides and conflicts with: `rediwm`
- Includes RediWM display-manager integration files.
- Installing the package does not enable the display-manager service or replace
  the currently selected display manager.

This is an independently maintained packaging recipe, not an official upstream
RediWM repository.
