# Maintainer: TDY <tdy@gmx.com>

pkgname=manokwari-git
pkgver=20121229
pkgrel=1
pkgdesc="A desktop shell for GNOME3"
arch=('i686' 'x86_64')
url="https://github.com/BlankOn/manokwari"
license=('GPL')
depends=('gconf' 'glib2>=2.12.0' 'gtk3>=3.0.8' 'atk>=2.0.0' 'libgee>=0.6.1' 'cairo>=1.10.2'
         'gnome-menus2>=2.30.5' 'libwnck3' 'libunique3' 'webkitgtk3>=1.3.0')
makedepends=('git' 'gnome-common' 'intltool>=0.25' 'vala')
install=gnome.install

_gitroot=https://github.com/BlankOn/manokwari.git
_gitname=manokwari

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  sed -i '/tests/d' configure.ac Makefile.am
  sed -i '/^manokwaridocdir =/ s/prefix/datadir/' Makefile.am
  export LDFLAGS+=' -lX11 -lm'
  ./autogen.sh
  ./configure --prefix=/usr \
              --sysconfdir=/etc \
              --localstatedir=/var \
              --disable-scrollkeeper
  make
}

package() {
  cd "$srcdir/$_gitname-build"
  make -j1 GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="$pkgdir/" install

  install -dm755 "$pkgdir/usr/share/gconf/schemas"
  gconf-merge-schema "$pkgdir/usr/share/gconf/schemas/$_gitname.schemas" \
    "$pkgdir"/etc/gconf/schemas/*.schemas
  rm -f "$pkgdir"/etc/gconf/schemas/*.schemas
}

# vim:set ts=2 sw=2 et:
