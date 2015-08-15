# GnuPG2 GIT version
# Maintainer: alphazo@gmail.com
# Based on official package maintained by Tobias Powalowski <tpowa@archlinux.org>
# Fixes from @nullren

pkgname=gnupg2-git
pkgver=20130627
pkgrel=1
pkgdesc="GNU Privacy Guard 2 - a PGP replacement tool. Development version. Do not use in production environments. Test new ECC algorithm by using --expert with --gen-key"
arch=('i686' 'x86_64')
depends=('libldap' 'curl' 'bzip2' 'zlib' 'libksba>=1.2' 'libgpg-error>=1.1' 'libgcrypt>=1.5'
	'pth' 'libusb-compat' 'libassuan-git' 'npth-git' 'texinfo' 'readline' 'pinentry')
license=('GPL')
url="http://www.gnupg.org/"
makedepends=('git' 'ghostscript' 'transfig' 'automake-1.11')
provides=("gnupg=${pkgver}" 'dirmngr')
conflicts=('gnupg2' 'gnupg' 'dirmngr')
replaces=('gnupg2' 'gnupg' 'dirmngr')
install=${pkgname}.install
sha1sums=('cc14d9d221fbfc44ff612677d6f4bd96b679b504')

_gitroot="git://git.gnupg.org/gnupg.git"
_gitname="gnupg"


build() {
 # cd ${srcdir}/gnupg-$pkgver
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
     #cd $_gitname && git pull origin
     cd $_gitname && git pull origin master
     msg "The local files are updated."
   else
     git clone $_gitroot $_gitname
     cd $_gitname
     fi

   msg "GIT checkout done or server timeout"
   msg "Starting make..."
   rm -rf "$srcdir/$_gitname-build"
   #git checkout gnupg-2.1.0beta3
   git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
   cd "$srcdir/$_gitname-build"
   AUTOMAKE_SUFFIX="-1.11" ./autogen.sh --force
   ./configure --enable-maintainer-mode --prefix=/usr --libexecdir=/usr/lib/gnupg #$EXTRAOPTS
   #sed -i 's/AM_CONFIG_HEADER/AC_CONFIG_HEADERS/g' configure.ac
   #sed -i 's/doc, yes/doc, no/g' configure.ac
   #autoreconf -fiv --force
   #LIBS=-llber ./configure --enable-maintainer-mode --prefix=/usr --libexecdir=/usr/lib/gnupg #$EXTRAOPTS
   make
}

package() {
  cd "$srcdir/$_gitname-build"
  make DESTDIR=${pkgdir} install
  # move conflicting files
  #mv ${pkgdir}/usr/share/gnupg{,2}
  #rm -f ${pkgdir}/usr/share/info/dir
  # Remove conflicting man file
  #rm -f ${pkgdir}/usr/share/man/man7/gnupg.7 
  ln -s gpg2 "${pkgdir}"/usr/bin/gpg
  #ln -s gpg2.1.gz "${pkgdir}"/usr/share/man/man1/gpg.1.gz 
}
