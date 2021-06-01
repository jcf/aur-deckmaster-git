# Maintainer: Christian Muehlhaeuser <muesli at gmail dot com>

pkgname=deckmaster-git
pkgver=r99.55628fd
pkgrel=1
pkgdesc="An application to control your Elgato Stream Deck"
arch=('x86_64' 'i686' 'armv6h' 'armv7h' 'aarch64')
url="https://github.com/muesli/${pkgname%-git}"
license=('MIT')
depends=('ttf-roboto')
makedepends=('git' 'go')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
source=($pkgname::"git://github.com/muesli/${pkgname%-git}.git"
        'deckmaster@.path'
        'deckmaster@.service'
        '60-elgato-stream-deck.rules')
sha256sums=('SKIP'
            'f6bbd3ace28aabd7fb9af80d7d99483c3ab3c193c298520a873d3945e882a62c'
            '6f0e6637ccdf34f7cf3cb9d165a181abbda8012a25fc50949f12853dd553bfca'
            '0344805537572dec2ae217e17fcc284a4544d1281d5d5edaf2b81b684ad836f3')

pkgver() {
    cd "$srcdir/$pkgname"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
    cd "$srcdir/$pkgname"

    local commit
    local extraflags
    commit=$(git rev-parse --short HEAD)
    extraflags="-X main.Version=${pkgver} -X main.CommitSHA=${commit}"

    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"
    export CGO_LDFLAGS="${LDFLAGS}"

    go build \
        -trimpath \
        -buildmode=pie \
        -mod=readonly \
        -modcacherw \
        -ldflags "${extraflags} -extldflags \"${LDFLAGS}\"" \
        -o "${pkgname%-git}" .
}

package() {
    cd "$srcdir/$pkgname"

    install -Dm755 "${pkgname%-git}" "$pkgdir/usr/bin/${pkgname%-git}"
    install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/${pkgname%-git}/LICENSE"

    install -Dm644 -t "${pkgdir}"/usr/lib/systemd/user/ \
      "${srcdir}"/deckmaster@.path "${srcdir}"/deckmaster@.service

    install -Dm644 -t "${pkgdir}"/usr/lib/udev/rules.d/ \
      "${srcdir}"/60-elgato-stream-deck.rules
}
