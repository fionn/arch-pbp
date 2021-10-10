# U-Boot: Pinebook Pro based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-pinebookpro
pkgver=2021.10
pkgrel=1
_tfaver=2.5
pkgdesc="U-Boot for Pinebook Pro"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
provides=('uboot')
conflicts=('uboot')
install=${pkgname}.install
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz"
        "0001-PBP-Fix-Panel-reset.patch"
        "0002-Correct-boot-order-to-be-USB-SD-eMMC.patch"
        "0003-rk3399-light-pinebook-power-and-standby-leds-during-early-boot.patch"
        "0001-fix-rk3399-suspend-correct-LPDDR4-resume-sequence.patch"
        "0002-fix-rockchip-rk3399-fix-dram-section-placement.patch")
sha256sums=('cde723e19262e646f2670d25e5ec4b1b368490de950d4e26275a988c36df0bd4'
            'ad8a2ffcbcd12d919723da07630fc0840c3c2fba7656d1462e45488e42995d7c'
            'c3ea09a18b766a3ce0728234b097b29e2ed610c7f04b138b7fba42e118a7ae33'
            '017d33aac55f8a5ed22170c97b4792ba755a4dad04f6c0cdd85119bbc81e87b3'
            '1b68afa7e70a19b809d4bf1710ad57e9fd7048bf5073887d0f6550a94a108ca4'
            '2cd375c5456e914208eb1b36adb4e78ee529bdd847958fb518a9a1be5b078b12'
            '52c3641b59422cb4174ac5c0c1d8617917ac05472d0d0a3437db128c077673fb')

prepare() {
  cd u-boot-${pkgver/rc/-rc}
  # Patches based on the work of dhivael and Nadia
  patch -Np1 -i "${srcdir}/0001-PBP-Fix-Panel-reset.patch"                                              # Fix Panel reset
  patch -Np1 -i "${srcdir}/0002-Correct-boot-order-to-be-USB-SD-eMMC.patch"                             #USB boot
  patch -Np1 -i "${srcdir}/0003-rk3399-light-pinebook-power-and-standby-leds-during-early-boot.patch"   #Orange LED
  cd ../trusted-firmware-a-$_tfaver
  patch -Np1 -i "${srcdir}/0001-fix-rk3399-suspend-correct-LPDDR4-resume-sequence.patch"                #Suspend fix
  patch -Np1 -i "${srcdir}/0002-fix-rockchip-rk3399-fix-dram-section-placement.patch"                   #GCC 11 fix
}

build() {
  cd trusted-firmware-a-$_tfaver
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make PLAT=rk3399
  cp build/rk3399/release/bl31/bl31.elf ../u-boot-${pkgver/rc/-rc}/
  cd ../u-boot-${pkgver/rc/-rc}
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make pinebook-pro-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Manjaro ARM"' >> .config
  echo 'CONFIG_USB_EHCI_HCD=n' >> .config
  echo 'CONFIG_USB_EHCI_GENERIC=n' >> .config
  echo 'CONFIG_USB_XHCI_HCD=n' >> .config
  echo 'CONFIG_USB_XHCI_DWC3=n' >> .config
  echo 'CONFIG_USB_DWC3=n' >> .config
  echo 'CONFIG_USB_DWC3_GENERIC=n' >> .config


  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"
  cp idbloader.img u-boot.itb  "${pkgdir}/boot/"
}
