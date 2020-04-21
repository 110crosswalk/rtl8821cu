# rtl8821cu
Wifi driver for rtl8811cu/rtl8821cu with monitor mode (v5.2.15)

### DKMS
Automatically install a kernel module when a new kernel gets installed
```
sudo apt install dkms
```
### Install
```
sudo ./dkms-install.sh
```
### Remove
```
sudo ./dkms-remove.sh
```
### Modeswitch
Just modify the file */lib/udev/rules.d/40-usb_modeswitch.rules* appending before the line *LABEL="modeswitch_rules_end"* with:
```
# Realtek 8811CU/8821CU Wifi AC USB
ATTR{idVendor}=="0bda", ATTR{idProduct}=="1a2b", RUN+="/usr/sbin/usb_modeswitch -K -v 0bda -p 1a2b"
```

### ARM architecture tweak for this driver (this solves compilation problem of this driver):
sudo cp /lib/modules/$(uname -r)/build/arch/arm/Makefile /lib/modules/$(uname -r)/build/arch/arm/Makefile.$(date +%Y%m%d%H%M)
sudo sed -i 's/-msoft-float//' /lib/modules/$(uname -r)/build/arch/arm/Makefile
sudo ln -s /lib/modules/$(uname -r)/build/arch/arm /lib/modules/$(uname -r)/build/arch/armv7l

### Monitor mode
Use the tool 'iw', please don't use other tools like 'airmon-ng'
```make ARCH="arm" CROSS_COMPILE=armv5tel-softfloat-linux-gnueabi- KSRC=/home/linux-master modules
iw dev wlan0 set monitor none


### Build
sudo apt-get install linux-headers-$(uname -r)
make ARCH="arm" CROSS_COMPILE=armv5tel-softfloat-linux-gnueabi- KSRC=/home/linux-master modules
```
