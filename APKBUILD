# Maintainer: YourLocalTechGuy <yourlocaldevguy@gmail.com>
_flavor="samsung-a22x"
pkgname=linux-$_flavor
pkgver=7.2
pkgrel=0
_commit="1787767cced5ee0eba8fccf17449352088c1d1c0"
pkgdesc="Samsung Galaxy A22 5G custom kernel (Clang/LLVM)"
arch="aarch64"
_carch="arm64"
url="https://github.com/YourLocalTechGuy/a22x-postmarketOS-kernel"
license="GPL-2.0-only"
options="!check !tracedeps
    pmb:cross-native
    pmb:kconfigcheck-community
"
makedepends="
    bash
    bison
    clang
    devicepkg-dev
    elfutils-dev
    findutils
    flex
    linux-headers
    lld
    llvm
    openssl-dev
    pahole
    perl
    postmarketos-installkernel
    python3
    zstd
"

_config="config-$_flavor.$arch"

# Source structure matching standard postmarketOS kernel packages
source="
    a22x-kernel-$_commit.tar.gz::https://github.com/YourLocalTechGuy/a22x-postmarketOS-kernel/archive/$_commit.tar.gz
    $_config
"
builddir="$srcdir/a22x-postmarketOS-kernel-$_commit"

prepare() {
    default_prepare
    cp "$srcdir/$_config" .config
}

build() {
    unset LDFLAGS
    make ARCH="$_carch" LLVM=1 \
        KBUILD_BUILD_VERSION="$((pkgrel + 1))-$_flavor"
}

package() {
    mkdir -p "$pkgdir"/boot

    # Install the compressed kernel image
    install -Dm644 "$builddir/arch/$_carch/boot/Image.gz" \
        "$pkgdir/boot/vmlinuz"

    # Install modules and dtbs using standard kernel targets
    make modules_install dtbs_install \
        ARCH="$_carch" \
        LLVM=1 \
        INSTALL_PATH="$pkgdir"/boot/ \
        INSTALL_MOD_PATH="$pkgdir"/usr \
        INSTALL_MOD_STRIP=1 \
        INSTALL_DTBS_PATH="$pkgdir"/boot/dtbs-tmp

    # Isolate your device's MediaTek DTB
    mkdir -p "$pkgdir"/boot/dtbs/mediatek
    cp -r "$pkgdir"/boot/dtbs-tmp/mediatek/mt6833-samsung-a22x.dtb "$pkgdir"/boot/dtbs/mediatek/
    rm -rf "$pkgdir"/boot/dtbs-tmp

    # Clean up host symlinks
    rm -f "$pkgdir"/lib/modules/*/build "$pkgdir"/lib/modules/*/source

    # Register kernel release descriptor for mkinitfs
    install -D "$builddir"/include/config/kernel.release \
        "$pkgdir"/usr/share/kernel/$_flavor/kernel.release
}

sha512sums="
1060ea2d3ceb084d2409311c0b771d9e1ed59faeccac9f7e6424697247f0ff2a72cac9a802ddad6f95ade17bcccde27659f865130cb7d71e5ec8c7c5b1d783fc  a22x-kernel-1787767cced5ee0eba8fccf17449352088c1d1c0.tar.gz
d258f79c92a3f62df798752ac24e7e07bcfad35e8ad2b97a251871d3974423c37f3484dabc224827f8e8e791f0b1a13701a31d8602d08ed92375ae956089f8d0  config-samsung-a22x.aarch64
"
