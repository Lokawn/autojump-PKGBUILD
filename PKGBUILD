# Maintainer: graysky <therealgraysky AT protonmail DOT com>
# Contributor: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jaroslav Lichtblau <svetlemodry@archlinux.org>
# Contributor: Geoffroy Carrier <geoffroy@archlinux.org>
# Contributor: Joël Schaerer <joel.schaerer@laposte.net>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=autojump
pkgver=22.5.3
pkgrel=9
pkgdesc="A faster way to navigate your filesystem from the command line"
arch=('any')
url="https://github.com/wting/autojump"
license=('GPL3')
depends=('python>=3.3')
conflicts=('shonenjump')
source=($pkgname-$pkgver.tar.gz::https://github.com/wting/$pkgname/archive/release-v$pkgver.tar.gz)
sha512sums=('d1dd3cbb67fda4e0a17ec5028b947faf46be8a95a6cd8418127b927f42bc95b71538a06658b38b479c77d147a6cd5e8cef77639ef538c7d449414c469c13f140')
install=readme.install

prepare() {
  cd $pkgname-release-v$pkgver
  sed -i "s:/env python:/python3:g" bin/$pkgname
}

package() {
  cd $pkgname-release-v$pkgver

  # https://wiki.archlinux.org/index.php/Python_package_guidelines#Using_site-packages
  local _site_packages=$(python -c "import site; print(site.getsitepackages()[0])")

  SHELL=/bin/bash ./install.py --destdir "${pkgdir}" \
    --prefix 'usr/' \
    --zshshare 'usr/share/zsh/site-functions'

  cd "${pkgdir}/usr/share/$pkgname"
  for i in $pkgname.*
    do ln -s "/usr/share/$pkgname/$i" "${pkgdir}/etc/profile.d/$i"
  done

  # FS#60929
  install -d "${pkgdir}/$_site_packages"
  mv ${pkgdir}/usr/bin/*.py "${pkgdir}/$_site_packages"
  python -m compileall -d /usr/lib "${pkgdir}/usr/lib"
  python -O -m compileall -d /usr/lib "${pkgdir}/usr/lib"

  # FS#49601
  install -d "${pkgdir}"/usr/share/fish/functions
  mv "${pkgdir}"/etc/profile.d/$pkgname.fish "${pkgdir}"/usr/share/fish/functions

  # https://github.com/joelthelion/autojump/pull/339
  sed -i "s:/usr/local/:/usr/:g" "${pkgdir}/etc/profile.d/$pkgname.sh"
  sed -i "s:/build/autojump/pkg/autojump/:/:g" "${pkgdir}/etc/profile.d/$pkgname.sh"

  # FS#43762
  sed -i '27,31d' "${pkgdir}/etc/profile.d/$pkgname.sh"
}
