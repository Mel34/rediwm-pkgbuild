# Maintainer: Local package build
_pkgname=rediwm
pkgname=rediwm-git
pkgver=0.1.7.r61.2ab4091
pkgrel=1
pkgdesc='Wayland compositor and desktop shell written in Zig'
arch=('x86_64')
url='https://github.com/oxydizer/rediwm'
license=('MIT' 'Apache-2.0')
depends=(
  'wlroots0.20'
  'wayland'
  'wayland-protocols'
  'xkeyboard-config'
  'pixman'
  'freetype2'
  'harfbuzz'
  'fontconfig'
  'librsvg'
  'gdk-pixbuf2'
  'libinput'
  'libpulse'
  'pipewire'
  'poppler-glib'
  'libseccomp'
  'libarchive'
  'libjpeg-turbo'
  'libpng'
  'libwebp'
  'readline'
  'glib2'
  'json-glib'
  'libtorrent-rasterbar'
  'boost-libs'
  'pam'
  'cups'
)
makedepends=(
  'git'
  'zig>=0.17.0'
  'pkgconf'
  'boost'
)
optdepends=(
  'xorg-xwayland: run X11 applications'
  'xdg-desktop-portal-wlr: screen sharing and portal integration'
  'foot: default terminal application'
  'brightnessctl: brightness controls'
)
provides=('rediwm')
conflicts=('rediwm')
source=("git+${url}.git")
sha256sums=('SKIP')

backup=('etc/rediwm/dm.conf'
        'etc/pam.d/rediwm-greeter'
        'etc/pam.d/rediwm-dm'
        'etc/pam.d/rediwm-dm-autologin')

pkgver() {
  cd "$_pkgname"
  git describe --long --tags --match 'v[0-9]*' |
    sed -E 's/^v//; s/-([0-9]+)-g/.r\1./'
}

build() {
  cd "$_pkgname"
  zig build -Doptimize=ReleaseSafe -j"$(nproc)" \
    --prefix "$srcdir/rediwm-build-prefix"
}
package() {
  cd "$_pkgname"

  # Copy the build already staged under $srcdir; never install to the host.
  install -d "$pkgdir/usr/local"
  cp -a "$srcdir/rediwm-build-prefix/." "$pkgdir/usr/local/"
  # These integration files belong in the system data directories.
  install -Dm644 \
    "$pkgdir/usr/local/share/xdg-desktop-portal/portals/rediwm.portal" \
    "$pkgdir/usr/share/xdg-desktop-portal/portals/rediwm.portal"
  rm "$pkgdir/usr/local/share/xdg-desktop-portal/portals/rediwm.portal"

  install -Dm644 \
    "$pkgdir/usr/local/share/polkit-1/actions/org.rediwm.accounts.policy" \
    "$pkgdir/usr/share/polkit-1/actions/org.rediwm.accounts.policy"
  rm "$pkgdir/usr/local/share/polkit-1/actions/org.rediwm.accounts.policy"

  install -Dm644 \
    "$pkgdir/usr/local/share/wayland-sessions/rediwm-release-safe.desktop" \
    "$pkgdir/usr/share/wayland-sessions/rediwm-release-safe.desktop"
  rm "$pkgdir/usr/local/share/wayland-sessions/rediwm-release-safe.desktop"
  install -Dm644 LICENSE-MIT "$pkgdir/usr/share/licenses/$pkgname/LICENSE-MIT"
  install -Dm644 LICENSE-APACHE "$pkgdir/usr/share/licenses/$pkgname/LICENSE-APACHE"

  # Arch-native display manager integration. Installing these files does not
  # enable the service or replace the currently selected display manager.
  install -Dm644 data/rediwm-dm.service \
    "$pkgdir/usr/lib/systemd/system/rediwm-dm.service"
  install -Dm644 data/rediwm-dm.sysusers \
    "$pkgdir/usr/lib/sysusers.d/rediwm-dm.conf"
  install -Dm644 data/rediwm-dm.tmpfiles \
    "$pkgdir/usr/lib/tmpfiles.d/rediwm-dm.conf"
  install -Dm644 data/rediwm-dm.conf \
    "$pkgdir/etc/rediwm/dm.conf"
  install -Dm644 data/pam/rediwm-greeter \
    "$pkgdir/etc/pam.d/rediwm-greeter"
  install -Dm644 data/pam/arch/rediwm-dm \
    "$pkgdir/etc/pam.d/rediwm-dm"
  install -Dm644 data/pam/arch/rediwm-dm-autologin \
    "$pkgdir/etc/pam.d/rediwm-dm-autologin"

}
