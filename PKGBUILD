# Maintainer: Frank1o3
pkgname=sklauncher
pkgver=4.0.29
pkgrel=1
pkgdesc="A custom Minecraft launcher"
arch=('x86_64')
url="https://skmedix.pl/"
license=('custom')

# System dependencies required to run the Electron app on CachyOS
depends=('gtk3' 'libnotify' 'libxss' 'libxtst' 'nss' 'alsa-lib' 'libappindicator-gtk3')

# squashfs-tools is needed to cleanly extract the AppImage without needing FUSE
makedepends=('squashfs-tools')

# The GitHub download link with the correct v-prefixed tag
source=("SKlauncher-${pkgver}-x86_64.AppImage::https://github.com/sklauncher/binaries/releases/download/v${pkgver}/SKlauncher-${pkgver}-x86_64.AppImage")
# The SHA256 hash you provided to ensure the file downloads correctly and isn't corrupted
sha256sums=('5e0b2f817940faaf96c310b69e207c66192f7c77cd27dd9e2751345fa65b846f')

prepare() {
    # Make the AppImage executable
    chmod +x "SKlauncher-${pkgver}-x86_64.AppImage"
    
    # Extract the AppImage using unsquashfs
    msg2 "Extracting AppImage..."
    unsquashfs -d squashfs-root "SKlauncher-${pkgver}-x86_64.AppImage"
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
    
    # 5. Create a wrapper script in /usr/bin to handle Wayland flags cleanly for Hyprland
    install -d "$pkgdir/usr/bin"
    cat > "$pkgdir/usr/bin/sklauncher" <<EOF
#!/bin/sh
exec /opt/sklauncher/sklauncher --enable-features=UseOzonePlatform --ozone-platform=wayland --wayland-text-input-version=3 "\$@"
EOF
    chmod 755 "$pkgdir/usr/bin/sklauncher"
    
    # 6. Install the desktop shortcut and point it to our wrapper script
    install -Dm644 squashfs-root/pl.skmedix.sklauncher.desktop "$pkgdir/usr/share/applications/sklauncher.desktop"
    # Replace the original Exec line with our wrapper script
    sed -i "s|Exec=.*|Exec=/usr/bin/sklauncher %U|g" "$pkgdir/usr/share/applications/sklauncher.desktop"
    
    # 7. Install the 512x512 icon so it shows up properly in your app launcher
    install -Dm644 squashfs-root/usr/share/icons/hicolor/512x512/apps/sklauncher.png \
        "$pkgdir/usr/share/icons/hicolor/512x512/apps/sklauncher.png"
}