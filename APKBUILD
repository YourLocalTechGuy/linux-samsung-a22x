# Maintainer: YourLocalTechGuy <yourlocaldevguy@gmail.com>
_flavor="samsung-a22x"
pkgname=linux-$_flavor
pkgver=7.2
pkgrel=0
_commit="d8e9901c4064dd491e7643cf001d5b32ac75a227"
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
355b461544f032d1d1c0ec88e16498fcdddc4dccfaeafa27fe31d40b909068b577e596a19539f9db1f0a02646cb589282b81be3578327a71f42f04d02bab4ce3  a22x-kernel-d8e9901c4064dd491e7643cf001d5b32ac75a227.tar.gz
35d0173b19934e7b1e3e247c777c92bff0db6755bdec78f8253b9b2542c83c42d5818938b588f49072493e727f494c5057b6ef93b0a123ecb819a391f3cd93a1  config-samsung-a22x.aarch64
"
