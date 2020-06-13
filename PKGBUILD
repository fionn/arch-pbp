# U-Boot: Pinebook Pro based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-pinebookpro
pkgver=2020.01
pkgrel=8
_tfaver=2.3
pkgdesc="U-Boot for Pinebook Pro"
arch=('aarch64')
url='https://git.eno.space/pbp-uboot.'
license=('GPL')
backup=('boot/extlinux/extlinux.conf')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
install=${pkgname}.install
_commit_atf=22d12c4148c373932a7a81e5d1c59a767e143ac2
source=("git+https://git.eno.space/pbp-uboot.git"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz"
        "extlinux.conf")
        #'0001-nvme-support.patch')
sha256sums=('SKIP'
            '37f917922bcef181164908c470a2f941006791c0113d738c498d39d95d543b21'
            'e29431ffeb7368983c74b7f7ea108f74e1363305231b0b5bb698884945ab0c5a')

prepare() {
  cd pbp-uboot
  #patch -Np1 -i "${srcdir}/0001-nvme-support.patch"
}

build() {
  cd trusted-firmware-a-$_tfaver
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make PLAT=rk3399
  cp build/rk3399/release/bl31/bl31.elf ../pbp-uboot
  cd ../pbp-uboot
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make pinebook_pro-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Manjaro ARM"' >> .config
  #echo 'CONFIG_PCIE_ROCKCHIP=y' >> .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd pbp-uboot

  mkdir -p "${pkgdir}/boot/extlinux"
  cp idbloader.img u-boot.itb  "${pkgdir}/boot"
  cp "${srcdir}"/extlinux.conf "${pkgdir}"/boot/extlinux
}
