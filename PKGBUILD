# Maintainer: Frank1o3
pkgname=sklauncher
pkgver=4.0.29
pkgrel=3
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
    "sklauncher.desktop::https://raw.githubusercontent.com/Frank1o3/sklauncher-pkg/main/sklauncher"
)

sha256sums=(
    '5e0b2f817940faaf96c310b69e207c66192f7c77cd27dd9e2751345fa65b846f'
    '0300add88520f333a1558bd98a42ca7ab6853cb62c2f5097689e8755447e09ad'
    'f7a878095276ee1d83fdb096bfe87f109ccfa8373ac29cdb92b290df66c76d4f'
)

prepare() {
    chmod +x "SKlauncher-${pkgver}-x86_64.AppImage"
    ./SKlauncher-${pkgver}-x86_64.AppImage --appimage-extract
}

package() {
    install -d "$pkgdir/opt/sklauncher"

    # Copy EVERYTHING exactly as extracted
    cp -a squashfs-root/. "$pkgdir/opt/sklauncher"

    # AppImage extracts restrictive permissions (700).
    # Packages installed under /opt must be world-readable/executable.
    find "$pkgdir/opt/sklauncher" -type d -exec chmod 755 {} +
    find "$pkgdir/opt/sklauncher" -type f -exec chmod 644 {} +

    # Restore executable bits
    chmod 755 \
        "$pkgdir/opt/sklauncher/AppRun" \
        "$pkgdir/opt/sklauncher/sklauncher" \
        "$pkgdir/opt/sklauncher/chrome_crashpad_handler"
    
    rm -f "$pkgdir/opt/sklauncher/pl.skmedix.sklauncher.desktop"

    # Remove bundled libraries but keep the directory
    if [[ -d "$pkgdir/opt/sklauncher/usr/lib" ]]; then
        find "$pkgdir/opt/sklauncher/usr/lib" -mindepth 1 -delete
    fi

    # Electron sandbox
    chown root:root "$pkgdir/opt/sklauncher/chrome-sandbox"
    chmod 4755 "$pkgdir/opt/sklauncher/chrome-sandbox"

    install -Dm644 "$srcdir/sklauncher" \
        "$pkgdir/usr/bin/sklauncher"

    install -Dm644 "$srcdir/sklauncher.desktop" \
        "$pkgdir/usr/share/applications/sklauncher.desktop"

    install -Dm644 \
        "$pkgdir/opt/sklauncher/usr/share/icons/hicolor/512x512/apps/sklauncher.png" \
        "$pkgdir/usr/share/icons/hicolor/512x512/apps/sklauncher.png"
}