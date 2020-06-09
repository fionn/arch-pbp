# U-Boot: Pinebook Pro based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-pinebookpro
pkgver=2020.07rc4
pkgrel=1
_tfaver=2.3
pkgdesc="U-Boot for Pinebook Pro"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/extlinux/extlinux.conf')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
install=${pkgname}.install
_commit_atf=22d12c4148c373932a7a81e5d1c59a767e143ac2
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz"
        "extlinux.conf")
sha256sums=('24a7b888e2c39cc93a8f5af5d5bf2eb0a9412f6992df71716c6fe233faef250a'
            '37f917922bcef181164908c470a2f941006791c0113d738c498d39d95d543b21'
            'e29431ffeb7368983c74b7f7ea108f74e1363305231b0b5bb698884945ab0c5a')

prepare() {
  cd u-boot-${pkgver/rc/-rc}
}

build() {
  cd trusted-firmware-a-$_tfaver
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make PLAT=rk3399
  cp build/rk3399/release/bl31/bl31.elf ../pbp-uboot
  cd ../u-boot-${pkgver/rc/-rc}
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make pinebook-pro-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Manjaro ARM"' >> .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"
  cp idbloader.img u-boot.itb  "${pkgdir}/boot"
  cp "${srcdir}"/extlinux.conf "${pkgdir}"/boot/extlinux
}
