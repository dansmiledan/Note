## env building
rpi-imager: flash os image to sdcard

### init raspberrypi zero



### connect to zero with use cable
- check if new interface exist
  ``` shell
  lusb
  # 
  ifconfig
  ```

### build kernel on Fedora host
download cross compile from: https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a/downloads
unzip and add bin to PATH
``` shell
# install neccessary software
sudo dnf install gmp-devel
sudo dnf install libmpc-devel

# choose defconfig
ARCH=arm CROSS_COMPILE=arm-none-linux-gnueabihf- make bcmrpi_defconfig

# menuconfig
ARCH=arm CROSS_COMPILE=arm-none-linux-gnueabihf- make menuconfig

# compile
ARCH=arm CROSS_COMPILE=arm-none-linux-gnueabihf- make -j4 zImage modules dtbs

# install modules, install_mod_path must be sdcard, copy will not work
ARCH=arm CROSS_COMPILE=arm-none-linux-gnueabihf-  INSTALL_MOD_PATH=../modules/  make modules_install
```

after building kernel, update the file
``` shell
sudo cp arch/arm/boot/broadcom/zImage ~/mnt/fat32/kernel.img
sudo cp arch/arm/boot/broadcom/dts/*.dtb ~/mnt/fat32/
```



## chp02 linux device driver model
three terms: devices, driver and bus



