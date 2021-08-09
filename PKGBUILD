pkgname=st

# version is defined by upstream package
pkgver=0.8.4
pkgver() {
	cd "${pkgname}"
	printf "%s.r%s.%s" "$(awk '/^VERSION =/ {print $3}' config.mk)" \
		"$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}
pkgrel=1  # release number on top of upstream package
epoch=1

pkgdesc="Modified ST terminal with scrolling and url handling"
arch=('i686' 'x86_64')
url='https://st.suckless.org'
license=('MIT')

depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
optdepends=('dmenu: feed urls to dmenu')

provides=("${pkgname}")
conflicts=("${pkgname}")

options=('zipman')

source=('git://github.com/radio-van/st')

sha1sums=('SKIP')


# called after source extraction, before the build
# useful for applying patches
prepare() {
    # `srcdir` is where all source files are extracted, e.g. ./src
	cd $srcdir/${pkgname}
    echo "preparing at $(pwd)"

	# skip terminfo which conflicts with ncurses
	sed -i '/tic /d' Makefile
}

# called for compiling source files before installation
build() {
	cd "${pkgname}"
    echo "building at $(pwd)"
	make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

# called for installation
package() {
	cd "${pkgname}"
    # `pkgdir` - dir with built bundles, e.g. ./pkg
    echo "installing at $(pwd)"
	make PREFIX=/usr DESTDIR="${pkgdir}" install
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 README.md "${pkgdir}/usr/share/doc/${pkgname}/README.md"
	install -Dm644 Xdefaults "${pkgdir}/usr/share/doc/${pkgname}/Xdefaults.example"
}
