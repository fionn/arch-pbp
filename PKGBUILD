# U-Boot: Pinebook Pro based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>
# Contributor: Dragan Simic <dsimic@buserror.io>

pkgname=uboot-pinebookpro
pkgver=2023.01
pkgrel=1
_tfaver=2.8
pkgdesc="U-Boot for Pine64 Pinebook Pro"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc' 'swig' 'python-setuptools')
provides=('uboot')
conflicts=('uboot')
replaces=('uboot-pinebookpro-bsp')
install=${pkgname}.install
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-${_tfaver}.tar.gz"
        "0001-PBP-Fix-panel-reset.patch"
        "0002-Correct-boot-order-to-be-USB-SD-eMMC.patch"
        "0003-Turn-power-and-standby-LEDs-on-early.patch"
        "0004-mmc-sdhci-allow-disabling-sdma-in-spl.patch"       # From list: https://patchwork.ozlabs.org/project/uboot/patch/20220222013131.3114990-3-pgwipeout@gmail.com/
        "0005-arm-dts-Work-around-daughterboard-issues.patch"   # Will be upstreamed by the author
        "0001-Revert-mmc-rockchip_sdhci-Correct-error-checking.patch"
        "0002-Revert-iopoll-Extend-read_poll_timeout-macro-to-supp.patch"
        "0003-Revert-rockchip-sdhci-Add-HS400-Enhanced-Strobe-supp.patch"
        "0004-Revert-rockchip-sdhci-Add-HS400-Enhanced-Strobe-supp.patch"
        "0005-Revert-rockchip-sdhci-Fix-RK3399-eMMC-PHY-power-cycl.patch"
        "0006-Revert-mmc-rockchip_sdhci-enable-strobe-line-for-HS4.patch"
        "0007-Revert-mmc-rockchip_sdhci-Add-support-for-RK3568.patch"
        "0008-Revert-mmc-rockchip_sdhci-add-phy-and-clock-config-f.patch") # The one we want actually want to revert
sha256sums=('69423bad380f89a0916636e89e6dcbd2e4512d584308d922d1039d1e4331950f'
            'df4e0f3803479df0ea4cbf3330b59731bc2efc2112c951f9adb3685229163af9'
            'c3ea09a18b766a3ce0728234b097b29e2ed610c7f04b138b7fba42e118a7ae33'
            '0565082c0c716a1d0a2dce2dbcb25046a805177af09d6511a44586add7a9d33e'
            'e82b207c56f41c0cc7b85f9057936816ea2ed58533e2dd2f6d665e28c415be60'
            '7014c3f1ada93536787a4ce30b484dfe651c339391bd46869c61933825a0edcc'
            'ff082f1bfc8666ca372d5e43924c47e60df637f5d2722b47a19b30a1306e8c74'
            'a7770945e77c3a824698ebf92cedc290f61a76855a0484ad84fba6b249ea9630'
            'ce7fa23d5009305b11f7d9ef6bb2603b36f7ca1780c8fcddc87bb46bb08da8ef'
            '4c0dd7bd02fe2a35e6c2af2364fa96637f2c731a38bc309579ab971ab430e0e5'
            'ea82e07213828199d3a1dd9f8ca1812ddff8a9513bfb2d8466bba16f5e9ee743'
            'e0cac4e1a22f0b357d874ef50fc3a74c81df8ab8ea51b9dd65226e192a1cbb42'
            '41cea222a6a9a724d138a20f81cf2e2b4f1d8e5475cb31844ec6c200b5d9a23d'
            '710a08487070b390fd587b284f05be05b5ea1ba4a47a248770339e4b7dbf8684'
            '003d9a020788d240d98a6c448d258f1c5cb29acd86ee1f4f9b04e1157b470aaf')

prepare() {
  cd u-boot-${pkgver/rc/-rc}

  patch -N -p1 -i "${srcdir}/0001-PBP-Fix-panel-reset.patch"                         # Panel reset fix
  patch -N -p1 -i "${srcdir}/0002-Correct-boot-order-to-be-USB-SD-eMMC.patch"        # Boot from USB first
  patch -N -p1 -i "${srcdir}/0003-Turn-power-and-standby-LEDs-on-early.patch"        # Orange status LED
  patch -N -p1 -i "${srcdir}/0004-mmc-sdhci-allow-disabling-sdma-in-spl.patch"       # RK3399 suspend/resume
  patch -N -p1 -i "${srcdir}/0005-arm-dts-Work-around-daughterboard-issues.patch"    # MicroSD issues

  # Reverts
  patch -Np1 -i "${srcdir}/0001-Revert-mmc-rockchip_sdhci-Correct-error-checking.patch"
  patch -Np1 -i "${srcdir}/0002-Revert-iopoll-Extend-read_poll_timeout-macro-to-supp.patch"
  patch -Np1 -i "${srcdir}/0003-Revert-rockchip-sdhci-Add-HS400-Enhanced-Strobe-supp.patch"
  patch -Np1 -i "${srcdir}/0004-Revert-rockchip-sdhci-Add-HS400-Enhanced-Strobe-supp.patch"
  patch -Np1 -i "${srcdir}/0005-Revert-rockchip-sdhci-Fix-RK3399-eMMC-PHY-power-cycl.patch"
  patch -Np1 -i "${srcdir}/0006-Revert-mmc-rockchip_sdhci-enable-strobe-line-for-HS4.patch"
  patch -Np1 -i "${srcdir}/0007-Revert-mmc-rockchip_sdhci-Add-support-for-RK3568.patch"
  patch -Np1 -i "${srcdir}/0008-Revert-mmc-rockchip_sdhci-add-phy-and-clock-config-f.patch" # This commits causes boot issues on rk3399
}

build() {
  # Avoid build warnings by editing a .config option in place instead of
  # appending an option to .config, if an option is already present
  update_config() {
    if ! grep -q "^$1=$2$" .config; then
      if grep -q "^# $1 is not set$" .config; then
        sed -i -e "s/^# $1 is not set$/$1=$2/g" .config
      elif grep -q "^$1=" .config; then
        sed -i -e "s/^$1=.*/$1=$2/g" .config
      else
        echo "$1=$2" >> .config
      fi
    fi
  }

  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS

  cd trusted-firmware-a-${_tfaver}

  echo -e "\nBuilding TF-A for Pine64 Pinebook Pro...\n"
  make PLAT=rk3399
  cp build/rk3399/release/bl31/bl31.elf ../u-boot-${pkgver/rc/-rc}/

  cd ../u-boot-${pkgver/rc/-rc}

  echo -e "\nBuilding U-Boot for Pine64 Pinebook Pro...\n"
  make pinebook-pro-rk3399_defconfig

  update_config 'CONFIG_IDENT_STRING' '" Manjaro Linux ARM"'
  update_config 'CONFIG_SPL_MMC_SDHCI_SDMA' 'n'

  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"
  install -D -m 0644 idbloader.img u-boot.itb -t "${pkgdir}/boot"
}
