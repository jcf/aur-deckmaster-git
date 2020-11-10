# Maintainer: Christian Muehlhaeuser <muesli at gmail dot com>

pkgname=deckmaster-git
pkgver=r51.61d81d1
pkgrel=1
pkgdesc="An application to control your Elgato Stream Deck"
arch=('x86_64' 'i686' 'armv6h' 'armv7h' 'aarch64')
url="https://github.com/muesli/deckmaster"
license=('MIT')
makedepends=('go' 'git')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
source=($pkgname::git://github.com/muesli/deckmaster.git)
sha256sums=('SKIP')

prepare() {
    export GOPATH="$srcdir/gopath"
    export GO111MODULE=on

    go clean -modcache
}

pkgver() {
    cd "$srcdir/$pkgname"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
    cd "$srcdir/$pkgname"

    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"
    export CGO_LDFLAGS="${LDFLAGS}"

    go build \
        -trimpath \
        -buildmode=pie \
        -mod=readonly \
        -modcacherw \
        -ldflags "-X main.Version=$pkgver -linkmode external -extldflags \"${LDFLAGS}\"" \
        -o "${pkgname%-git}" .

    go clean -modcache
}

package() {
    cd "$srcdir/$pkgname"

    install -Dm755 "${pkgname%-git}" "$pkgdir/usr/bin/${pkgname%-git}"
    install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/${pkgname%-git}/LICENSE"
}