# Check specs of the device

```bash
uname -a
cat /proc/version
cat /etc/os-release
lsb_release -a
cat /proc/cpuinfo
cat /sys/firmware/devicetree/model
lscpu
free -h
df -h
lsblk
ls /sys/class/power_supply/
cat /proc/meminfo
vcgencmd version
dpkg --print-architecture
lsmod | head -20
```