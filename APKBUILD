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
00f4c60544055d4c8c4cc8538fb24ac62096e489f40eb7abb8da7aae1bbfde6be70e5c25be872efc8a07f5971fa2d4769065ec2b6dc3ea70c4c8423c29fc3d7a  Image.gz
7dc7de1f976149ecce263764f870c76262338c13516d91222495141a6b01bb8aaadfde20f3e0b7227cf7f3e9a0ff9b382419a0c6f5672ce4ac87e33f3d821b42  samsung-a22x.dtb
"
