# Maintainer of this PKGBUILD file: Martino Pilia <martino.pilia@gmail.com>
_name=openvslam
pkgname=${_name}-git
pkgver=r475.c7db4f6
pkgrel=1
pkgdesc="A Versatile Visual SLAM Framework"
arch=('x86_64')
url="https://github.com/xdspacelab/openvslam"
license=('BSD2')
depends=(
	'dbow2-openvslam-git'
	'eigen'
	'g2o'
	'intel-tbb'
	'metis'
	'openblas'
	'opencv'
	'openmp'
	'pangolin'
	'suitesparse'
	'yaml-cpp'
)
makedepends=(
	'cmake'
	'git'
)
provides=('openvslam')
source=("git+$url"
        "missing-include.patch::$url/commit/75699f2d842ee13eb3306653383599c25da89f7c.patch")
sha256sums=('SKIP'
            '32550aef06733b7f61e7bd658ba6ac1c4c6363a22f6797f7b942f29a62d3015e')

pkgver() {
	cd "${srcdir}/openvslam"
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
	cd "${srcdir}/openvslam"

	git am ../missing-include.patch

	[ ! -d build ] || rm -rf build
	mkdir build && cd build
	cmake .. \
		-DCMAKE_BUILD_TYPE:STRING='Release' \
		-DCMAKE_INSTALL_PREFIX:PATH='/usr' \
		-DCMAKE_CXX_FLAGS:STRING='-w' \
		-DUSE_PANGOLIN_VIEWER:BOOL=ON \
		-DINSTALL_PANGOLIN_VIEWER:BOOL=ON
}

build() {
	cd "${srcdir}/openvslam/build"
	make
}

package() {
	cd "${srcdir}/openvslam"
	make -C build DESTDIR="$pkgdir/" install

	mkdir "$pkgdir/usr/bin/"
	cp build/run* "$pkgdir/usr/bin/"

	install -D -m644 \
		"LICENSE" \
		"${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

