# Contributor: Larry Hajali <larryhaja [at] gmail [dot] com>

pkgname=sslsniff
pkgver=0.8
pkgrel=3
pkgdesc="sslsniff is designed to MITM all SSL connections on a LAN and dynamically generates certs for the domains that are being accessed on the fly."
arch=('i686' 'x86_64')
url="http://www.thoughtcrime.org/software/sslsniff/"
license=('custom')
depends=('boost' 'log4cpp' 'dsniff')
source=("http://www.thoughtcrime.org/software/${pkgname}/${pkgname}-${pkgver}.tar.gz"
        "boost-1.47.patch"
        "gcc-4.7-link-fix.patch"
        "fix-compatibility-with-gcc49.patch")
md5sums=('030fe31af33c22a932393c7a5f33bb2e'
         '439f5cd6a853e6b279a4cb1a89cec54d'
         '19894027f5896d2f69066dddb76de17f'
         '23427e47e8c4656773482774d0c182f4')

build()
{
  cd $pkgname-$pkgver

  # Boost 1.47 patch
  patch -p0 < "${srcdir}"/boost-1.47.patch
  # Fix linking with libraries in gcc 4.7.x
  patch -p1 < "${srcdir}"/gcc-4.7-link-fix.patch
  # Fix for gcc 4.9
  # https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=746915
  patch -p1 < "${srcdir}"/fix-compatibility-with-gcc49.patch

  ./configure --prefix=/usr --disable-dependency-tracking
  make || return 1
}

package()
{
  cd $pkgname-$pkgver
  make install DESTDIR="${pkgdir}"

  # Package some example material.
  install -d -m 0755 "${pkgdir}"/usr/share/${pkgname}/{updates,certs}
  install -m 0644 leafcert.pem IPSCACLASEA1.crt "${pkgdir}"/usr/share/$pkgname
  install -m 0644 updates/*xml "${pkgdir}"/usr/share/$pkgname/updates
  install -m 0644 certs/* "${pkgdir}"/usr/share/$pkgname/certs
  install -D -m 0644 README "${pkgdir}"/usr/share/doc/${pkgname}/README

  # Install license.
  install -D -m 0644 COPYING "${pkgdir}"/usr/share/licenses/$pkgname/LICENSE
}
