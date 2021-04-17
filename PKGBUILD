# U-Boot: Pinebook Pro based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-pinebookpro
pkgver=2021.04
pkgrel=1
_tfaver=2.4
pkgdesc="U-Boot for Pinebook Pro"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
provides=('uboot')
conflicts=('uboot')
install=${pkgname}.install
_commit_atf=22d12c4148c373932a7a81e5d1c59a767e143ac2
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz"
        "0001-Add-regulator-needed-for-usage-of-USB.patch"
        "0002-Correct-boot-order-to-be-USB-SD-eMMC.patch"
        "0003-rk3399-light-pinebook-power-and-standby-leds-during-early-boot.patch")
sha256sums=('0d438b1bb5cceb57a18ea2de4a0d51f7be5b05b98717df05938636e0aadfe11a'
            'bf3eb3617a74cddd7fb0e0eacbfe38c3258ee07d4c8ed730deef7a175cc3d55b'
            'bec7197f9dac0e146ff9f7d47c66d69e37b136a00aa0637afbad5f2f8f57b674'
            'fec8f32af8e2a9dd6f1d8dcc83453ebded74786de03a9be14823261de7421bf3'
            '1b68afa7e70a19b809d4bf1710ad57e9fd7048bf5073887d0f6550a94a108ca4')

prepare() {
  cd u-boot-${pkgver/rc/-rc}
  # Patches based on the work of dhivael and Nadia
  patch -Np1 -i "${srcdir}/0001-Add-regulator-needed-for-usage-of-USB.patch" # USB boot
  patch -Np1 -i "${srcdir}/0002-Correct-boot-order-to-be-USB-SD-eMMC.patch" #USB boot
  patch -Np1 -i "${srcdir}/0003-rk3399-light-pinebook-power-and-standby-leds-during-early-boot.patch" #Orange LED
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
