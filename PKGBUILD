# Maintainer: Frank1o3
pkgname=sklauncher
pkgver=4.0.29
pkgrel=2
pkgdesc="A custom Minecraft launcher"
arch=('x86_64')
url="https://skmedix.pl/"
license=('custom')

# System dependencies required to run the Electron app on CachyOS
depends=('gtk3' 'libnotify' 'libxss' 'libxtst' 'nss' 'alsa-lib' 'libappindicator-gtk3')

# 1. The AppImage from the official binaries repo
# 2. Your custom .desktop file from your GitHub repo
# (Note: If your default branch is 'master' instead of 'main', change 'main' to 'master' below)
source=(
    "SKlauncher-${pkgver}-x86_64.AppImage::https://github.com/sklauncher/binaries/releases/download/v${pkgver}/SKlauncher-${pkgver}-x86_64.AppImage"
    "sklauncher.desktop::https://raw.githubusercontent.com/Frank1o3/sklauncher-pkg/main/sklauncher.desktop"
    "sklauncher::https://raw.githubusercontent.com/Frank1o3/sklauncher-pkg/main/sklauncher"
)

# SHA256 hashes. 
# The first is the AppImage hash. 
# The second is for the desktop file (paste the hash you got from step 2 here)
sha256sums=(
    '5e0b2f817940faaf96c310b69e207c66192f7c77cd27dd9e2751345fa65b846f'
    '8a9452dcca7ab3bac222ea3f81c207bbb324a368fe6341814b31c1810c78e693'
    'a8f35eea60bef7f0a666c67b90cc8c4814049891b32269a0be9817dc1be0dcb3'
)

prepare() {
    # Make the AppImage executable
    chmod +x "SKlauncher-${pkgver}-x86_64.AppImage"
    
    # Extract the AppImage using its built-in extractor
    msg2 "Extracting AppImage..."
    ./SKlauncher-${pkgver}-x86_64.AppImage --appimage-extract
}

package() {
    # 1. Create the installation directory in /opt
    install -d "$pkgdir/opt/sklauncher"
    
    # 2. Copy all extracted files to /opt
    cp -a squashfs-root/* "$pkgdir/opt/sklauncher/"
    
    # 3. Remove the bundled "old Ubuntu" libraries to force CachyOS system packages
    rm -rf "$pkgdir/opt/sklauncher/usr/lib"
    
    # 4. Fix Electron sandbox permissions (Required for Electron apps to not crash)
    chmod 4755 "$pkgdir/opt/sklauncher/chrome-sandbox"
    
    # 5. Install the wrapper script from the downloaded source
    install -Dm755 "$srcdir/sklauncher" "$pkgdir/usr/bin/sklauncher"
    
    # 6. Install your custom desktop shortcut directly from the downloaded source
    install -Dm644 "$srcdir/sklauncher.desktop" "$pkgdir/usr/share/applications/sklauncher.desktop"
    
    # 7. Install the 512x512 icon so it shows up properly in your app launcher
    install -Dm644 squashfs-root/usr/share/icons/hicolor/512x512/apps/sklauncher.png \
        "$pkgdir/usr/share/icons/hicolor/512x512/apps/sklauncher.png"
}