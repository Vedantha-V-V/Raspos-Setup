## Step 1: Installation

```bash
    sudo apt update
    sudo apt install -y ca-certificates curl gnupg lsb-release
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### Tailored to Trixie:

```bash
echo "deb [arch=armhf signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian trixie stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Verify + User Access

```bash
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```

## Step 2: Error Debugging

### Diagnosis

```bash
systemctl status docker.service
journalctl -xeu docker.service | tail -20
```
### Top fixes

```bash
# 1. Kill conflicts (90% win)
sudo systemctl stop containerd docker
sudo rm -rf /var/lib/containerd /var/lib/docker
sudo systemctl daemon-reload

# 2. Trixie deps (iptables fix)
sudo apt install iptables containerd.io

# 3. Restart
sudo systemctl start docker
docker run hello-world
```

### If cgroup v2 error (Trixie kernel quirk):

```bash
sudo nano /etc/default/grub
```
- Add systemd.unified_cgroup_hierarchy=0 to GRUB_CMDLINE_LINUX_DEFAULT, then:

```bash
sudo update-grub && sudo reboot
```


### Docker Fix Final

```bash
# Kill bad state
sudo systemctl stop docker containerd || true
sudo rm -rf /var/lib/docker /var/lib/containerd

# Trixie iptables fix (nft → legacy)
sudo apt install iptables iptables-legacy
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy

# Reload & deps
sudo systemctl daemon-reload
sudo apt install containerd.io

# Cgroup v1 force (your 5.17 kernel)
sudo nano /boot/firmware/cmdline.txt
Add to end: systemd.unified_cgroup_hierarchy=0 cgroup_no_v1=all

# Reboot
sudo reboot

# Post reboot
sudo systemctl start docker
docker run hello-world
```

### Docker Fix

```bash
sudo systemctl stop docker docker.socket
sudo rm -rf /var/lib/docker /var/lib/containerd /run/containerd /run/docker*

sudo apt install iptables iptables-legacy containerd.io -y
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy

sudo systemctl daemon-reload
sudo systemctl start containerd
sudo systemctl start docker
```