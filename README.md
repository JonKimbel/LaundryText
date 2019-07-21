# LaundryText

An ESP8266-based project that text messages you when your laundry is done.

## Setup

Based on [these docs](https://github.com/esp8266/esp8266-wiki/wiki/Toolchain).

## Install prerequisites

```
apt-get install git autoconf build-essential gperf bison flex texinfo libtool libncurses5-dev wget gawk libc6-dev-amd64 python-serial libexpat-dev libtool-bin automake
```

## Set up directory for toolchain

```
mkdir /opt/Espressif
chown $USER /opt/Espressif/
```

## Install crosstool-NG

```
cd /opt/Espressif
git clone -b lx106 git://github.com/jcmvbkbc/crosstool-NG.git
cd crosstool-NG
```

### Fix the branch of crosstool-NG you just downloaded

Apply the change [here](https://github.com/crosstool-ng/crosstool-ng/pull/529/commits/3bb2bcd2a328fb56332398783e91899ce3bc3556).

### Build crosstool-NG

```
./bootstrap && ./configure --prefix=`pwd` && make && make install
./ct-ng xtensa-lx106-elf
./ct-ng build
PATH=$PWD/builds/xtensa-lx106-elf/bin:$PATH
```

## Downloading the SDK

```
cd /opt/Espressif
wget -O esp_iot_sdk_v0.9.3_14_11_21.zip https://github.com/esp8266/esp8266-wiki/raw/master/sdk/esp_iot_sdk_v0.9.3_14_11_21.zip
wget -O esp_iot_sdk_v0.9.3_14_11_21_patch1.zip https://github.com/esp8266/esp8266-wiki/raw/master/sdk/esp_iot_sdk_v0.9.3_14_11_21_patch1.zip
unzip esp_iot_sdk_v0.9.3_14_11_21.zip
unzip esp_iot_sdk_v0.9.3_14_11_21_patch1.zip
mv esp_iot_sdk_v0.9.3 ESP8266_SDK
mv License ESP8266_SDK/
```

### Reconfiguring the SDK to use xtensa

```
cd /opt/Espressif/ESP8266_SDK
sed -i -e 's/xt-ar/xtensa-lx106-elf-ar/' -e 's/xt-xcc/xtensa-lx106-elf-gcc/' -e 's/xt-objcopy/xtensa-lx106-elf-objcopy/' Makefile
mv examples/IoT_Demo .
```

### Installing xtensa libraries and headers (?)

```
cd /opt/Espressif/ESP8266_SDK
wget -O lib/libc.a https://github.com/esp8266/esp8266-wiki/raw/master/libs/libc.a
wget -O lib/libhal.a https://github.com/esp8266/esp8266-wiki/raw/master/libs/libhal.a
wget -O include.tgz https://github.com/esp8266/esp8266-wiki/raw/master/include.tgz
tar -xvzf include.tgz
```

### Installing ESP image tool

```
cd /opt/Espressif
wget -O esptool_0.0.2-1_i386.deb https://github.com/esp8266/esp8266-wiki/raw/master/deb/esptool_0.0.2-1_i386.deb
dpkg -i esptool_0.0.2-1_i386.deb
```

### Installing ESP upload tool

```
cd /opt/Espressif
git clone https://github.com/themadinventor/esptool esptool-py
ln -s $PWD/esptool-py/esptool.py crosstool-NG/builds/xtensa-lx106-elf/bin/
```
