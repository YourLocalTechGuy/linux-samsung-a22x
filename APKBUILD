# Maintainer: YourLocalTechGuy <yourlocaldevguy@gmail.com>
pkgname=linux-samsung-a22x
pkgver=4.14.186
pkgrel=0
pkgdesc="Samsung Galaxy A22 5G prebuilt kernel"
arch="aarch64"
url="https://postmarketos.org"
license="GPL-2.0-only"
options="!strip !check !tracedeps !archcheck"
source="
	Image.gz
	samsung-a22x.dtb
"
_flavor="samsung-a22x"
build() {
	true
}

#package() {
#	install -Dm644 "$srcdir/Image.gz" "$pkgdir/boot/vmlinuz-$_flavor"
# Satisfy postmarketos-mkinitfs kernel release discovery
#	mkdir -p "$pkgdir/usr/share/kernel/$_flavor"
#	echo "$pkgver" > "$pkgdir/usr/share/kernel/$_flavor/kernel.release"
#}
package() {
	# Install kernel binary
	install -Dm644 "$srcdir/Image.gz" "$pkgdir/boot/vmlinuz-$_flavor"

	# Install DTB inside the expected dtbs directory
	install -Dm644 "$srcdir/samsung-a22x.dtb" "$pkgdir/boot/dtbs-$_flavor/samsung-a22x.dtb"

	# Register release file for postmarketos-mkinitfs
	mkdir -p "$pkgdir/usr/share/kernel/$_flavor"
	echo "$pkgver" > "$pkgdir/usr/share/kernel/$_flavor/kernel.release"
}
sha512sums="
3148cf34c568ec22f7839e3a3b127f173bc9f782bdc10f0f7e14ebf19e733da614a7d40f1f8d67a1cee6908b752c672205ae597bf0a0ec839f1569322964126c  Image.gz
7dc7de1f976149ecce263764f870c76262338c13516d91222495141a6b01bb8aaadfde20f3e0b7227cf7f3e9a0ff9b382419a0c6f5672ce4ac87e33f3d821b42  samsung-a22x.dtb
"
