# Maintainer: YourLocalTechGuy <yourlocaldevguy@gmail.com>
_flavor="samsung-a22x"
pkgname=linux-$_flavor
pkgver=7.2
pkgrel=0
_commit="019a4db651e5d3d2ef194906188414f14f2a6264"
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
3ac4133ce9c57f53e4afacd4779ec9328d3bdf04ce3ebbbd5387854065942f4548e916ef80d248fddb4269445c824a2177b40f4f43915e800e8cf02edda9cfa8  a22x-kernel-019a4db651e5d3d2ef194906188414f14f2a6264.tar.gz
0c6d6d6980d12a7b47650f5979d9e0ce8f65b4884498ab1975b54d09ecc6479a89737bb354459435eff1fbde23d9893187081199448ed8fe5aade006b76112f3  config-samsung-a22x.aarch64
"
