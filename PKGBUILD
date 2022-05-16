# Maintainer: Aerian <wo199710@hotmail.com>
# Contributor: Caleb Maclennan <caleb@alerque.com> Alfin Bakhtiar Ilhami <alfin at nuclea dot id>

pkgname=marktext-bin
_pkgname=${pkgname%-bin}
pkgver=0.17.1
pkgrel=1
pkgdesc='A simple and elegant open-source markdown editor that focused on speed and usability'
arch=(x86_64)
url=https://marktext.app
license=(MIT)
provides=("$_pkgname")
conflicts=("$_pkgname")
_electron=electron
depends=(gtk3
         libsecret
         libxkbfile
         libxss
         nss
         "$_electron")
_source() {
	local _github=https://github.com/bigshans/marktext
    echo marktext.sh
	echo $pkgname-$pkgver.tar.gz::$_github/releases/download/v$pkgver/$_pkgname-x64.tar.gz
	echo $pkgname-$pkgver.desktop::$_github/raw/v$pkgver/resources/linux/$_pkgname.desktop
	for s in 16 24 32 48 64 128 256 512; do
		echo $pkgname-${s}x${s}.png::$_github/raw/v$pkgver/resources/icons/${s}x${s}/$_pkgname.png
	done
}
source=($(_source))
sha256sums=('8f37f164a642a536b75f54b49e7c7a7c1e4d355a91dd8ece4cab6a95b42d369e'
            '0fc800d8292213fff5f8ca748294d9791430eb0f8fff49fd2f2a3c1f34088e42'
            '95c55fae2e35c1b022d69736e496b04b24caba9cb7d7a7d4613076ea85d2b7cf'
            '27ef0b9185f38bdf516db32fa8900e3bfd182937bb14f63a978713d74ad97fa2'
            'f67f6826499b5fa25a931b706a7d500972c049fb23f406f4692206dfe1a302fc'
            'f6a3e0673b78d9e04e60afba4ed05ea9967c52308175a80f07f06b1125bafebe'
            'e2f07abbbed4bc6f146cef52958ebec5ec8ed6e606a884934430d3525a2bbae7'
            '0e72f017950c42e78355e71d80fc9e52a23dd1158f6d1e74a4c7a8c67bd599ac'
            '5dcee26e0a80b78534a71af0b6e5b425e0e030f31c1880699923999b22848536'
            'd00f87911e70a76a4a9eb13da40e535fd2aa99486c768c1906156b6ba5df7c1b'
            'bb55503655caa407094514c9f086aca13ebd66bd178bfe3394c63464a66ba727')

prepare() {
	sed -i -e "s|Exec=.*|Exec=/usr/bin/marktext %F|" \
		"$pkgname-$pkgver.desktop"
	sed -e "s/@ELECTRON@/$_electron/" "../$_pkgname.sh" > "$_pkgname"
}

package() {
    install -Dm0755 -t "$pkgdir/usr/bin" "$_pkgname"
    local _dist=marktext-x64/resources
	install -Dm0644 -t "$pkgdir/usr/lib/$_pkgname/" "$_dist/app.asar"
	cp -a "$_dist"/app.asar.unpacked "$pkgdir/usr/lib/$_pkgname/"
	local _rg_path="$pkgdir/usr/lib/$_pkgname/app.asar.unpacked/node_modules/vscode-ripgrep/bin/"
	mkdir -p $_rg_path
	ln -sf /usr/bin/rg "$_rg_path"
	install -Dm644 "$pkgname-$pkgver.desktop" \
		"$pkgdir/usr/share/applications/$_pkgname.desktop"
	for s in 16 24 32 48 64 128 256 512; do
		install -Dm644 "$pkgname-${s}x${s}.png" \
			"$pkgdir/usr/share/icons/hicolor/${s}x${s}/apps/$_pkgname.png"
	done
	cd "$_pkgname-x64"
	install -Dm644 -t "$pkgdir/usr/share/licenses/$_pkgname" \
		{LICENSE,LICENSE.electron.txt,LICENSES.chromium.html}
}
