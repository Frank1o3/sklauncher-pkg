# Maintainer: Frank1o3

pkgname=sklauncher
pkgver=4.0.29
pkgrel=4
pkgdesc="A custom Minecraft launcher"
arch=('x86_64')
url="https://skmedix.pl/"
license=('custom')

depends=(
    'gtk3'
    'libnotify'
    'libxss'
    'libxtst'
    'nss'
    'alsa-lib'
    'libappindicator-gtk3'
)

source=(
    "SKlauncher-${pkgver}-x86_64.AppImage::https://github.com/sklauncher/binaries/releases/download/v${pkgver}/SKlauncher-${pkgver}-x86_64.AppImage"
    "sklauncher.desktop::https://raw.githubusercontent.com/Frank1o3/sklauncher-pkg/main/sklauncher.desktop"
    "sklauncher::https://raw.githubusercontent.com/Frank1o3/sklauncher-pkg/main/sklauncher"
)

sha256sums=(
    '5e0b2f817940faaf96c310b69e207c66192f7c77cd27dd9e2751345fa65b846f'
    '0300add88520f333a1558bd98a42ca7ab6853cb62c2f5097689e8755447e09ad'
    'c42fb20baafd407435a28cc95e6aa528512e46a5630a444579011b5bb74e42b2'
)

prepare() {
    chmod +x "SKlauncher-${pkgver}-x86_64.AppImage"
    chmod +x "$srcdir/sklauncher"

    ./SKlauncher-${pkgver}-x86_64.AppImage --appimage-extract
}

package() {
    install -d "$pkgdir/opt/sklauncher"

    # Preserve original permissions (IMPORTANT)
    cp -a --no-preserve=ownership squashfs-root/. "$pkgdir/opt/sklauncher"

    # Ensure directories are accessible system-wide
    find "$pkgdir/opt/sklauncher" -type d -exec chmod 755 {} +

    # Remove upstream desktop file (we provide our own)
    rm -f "$pkgdir/opt/sklauncher/"*.desktop

    # Electron sandbox (required for non-root execution)
    if [[ -f "$pkgdir/opt/sklauncher/chrome-sandbox" ]]; then
        chown root:root "$pkgdir/opt/sklauncher/chrome-sandbox"
        chmod 4755 "$pkgdir/opt/sklauncher/chrome-sandbox"
    fi

    # Install launcher wrapper
    install -Dm755 "$srcdir/sklauncher" \
        "$pkgdir/usr/bin/sklauncher"

    # Install desktop entry
    install -Dm644 "$srcdir/sklauncher.desktop" \
        "$pkgdir/usr/share/applications/sklauncher.desktop"

    # Install icon
    install -Dm644 \
        "$pkgdir/opt/sklauncher/usr/share/icons/hicolor/512x512/apps/sklauncher.png" \
        "$pkgdir/usr/share/icons/hicolor/512x512/apps/sklauncher.png"
}