## Installations

### Code editors

```bash
sudo apt install -y neovim micro code-server
```

### Tablet Like Feel

```bash
# Ultra-light reader (20MB RAM)
sudo apt install foliate evince zathura

# Touch-optimized
sudo apt install matchbox-window-manager

# APK testing via Waydroid (works!)
# Install Anbox instead (APK testing, lighter)
sudo apt install snapd
sudo snap install --devmode --beta anbox

# Test APK
anbox app-install ~/test.apk
```