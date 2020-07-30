# U-Boot: Pinebook Pro based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-pinebookpro
pkgver=2020.07
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
        "0001-Add-regulator-needed-for-usage-of-USB.patch"
        "0002-Correct-boot-order-to-be-USB-SD-eMMC.patch"
        "0003-rk3399-light-pinebook-power-and-standby-leds-during-early-boot.patch"
        "extlinux.conf")
sha256sums=('c1f5bf9ee6bb6e648edbf19ce2ca9452f614b08a9f886f1a566aa42e8cf05f6a'
            '37f917922bcef181164908c470a2f941006791c0113d738c498d39d95d543b21'
            'bec7197f9dac0e146ff9f7d47c66d69e37b136a00aa0637afbad5f2f8f57b674'
            'fec8f32af8e2a9dd6f1d8dcc83453ebded74786de03a9be14823261de7421bf3'
            '1b68afa7e70a19b809d4bf1710ad57e9fd7048bf5073887d0f6550a94a108ca4'
            'f23954d0ed28a2e243847adb7746bb9e20a6e52931df79c871f03c51cbe12c39')

prepare() {
  cd u-boot-${pkgver/rc/-rc}
  # Patches based on the work of dhivael and Nadia
  patch -Np1 -i "${srcdir}/0001-Add-regulator-needed-for-usage-of-USB.patch" # USB boot
  patch -Np1 -i "${srcdir}/0002-Correct-boot-order-to-be-USB-SD-eMMC.patch" #USB boot
  patch -Np1 -i "${srcdir}/0003-rk3399-light-pinebook-power-and-standby-leds-during-early-boot.patch" #Orange LED
  sed -i s/"CONFIG_BOOTDELAY=3"/"CONFIG_BOOTDELAY=0"/g configs/pinebook-pro-rk3399_defconfig
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
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"
  cp idbloader.img u-boot.itb  "${pkgdir}/boot/"
  cp "${srcdir}"/extlinux.conf "${pkgdir}"/boot/extlinux/
}
