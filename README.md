# NRF24L01_Arduino_RaspberryPi

- [NRF24L01_Arduino_RaspberryPi](#nrf24l01-arduino-raspberrypi)
  * [NRF24L01](#nrf24l01)
  * [Arduino](#arduino)
    + [Kết nối dây và test module RF](#k-t-n-i-d-y-v--test-module-rf)
  * [Raspberry Pi](#raspberry-pi)
    + [Kết nối Wifi và bật SSH không cần màn hình](#k-t-n-i-wifi-v--b-t-ssh-kh-ng-c-n-m-n-h-nh)
    + [Cài nodejs](#c-i-nodejs)
    + [Cài đặt thư viện NRF24L01 trên Raspberry Pi](#c-i---t-th--vi-n-nrf24l01-tr-n-raspberry-pi)
    + [Kết nối dây](#k-t-n-i-d-y)

## NRF24L01

![alt text](DOCS/NRF24L01_pro2_acoptex.jpg)

## Arduino

Thư viện: `DOCS/RF24.zip`

Thêm thư viện vào arduino: `Sketch >> Include Library >> Add .Zip Library... >> chọn thư viện RF24.zip`

### Kết nối dây và test module RF

![alt text](DOCS/NRF24L01_pro6_acoptex.jpg)

- Nạp firmware `Arduino/TX_GettingStarted` vào board phát.
- Nạp firmware `Arduino/RX_GettingStarted` vào board thu.

![alt text](DOCS/getting_started.png)

Bật Serial Monitor trên 2 board Arduino. Nếu kết quả trên 2 monitor nhận được như hình trên thì OK. Nếu không, hãy kiểm tra lại kết nối dây giữa arduino và module RF (trên cả board thu và board phát).

## Raspberry Pi

### Kết nối Wifi và bật SSH không cần màn hình

1. Ghi image vào trong thẻ nhớ

2. Vào trong thư mục boot của thẻ nhớ, tạo file `ssh` với nội dung là 01 dấu cách(space).

3. Kết nối wifi

tạo file `wpa_supplicant.conf` trong thư mục boot và nhập nội dung:

```bash
country=VN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="NETWORK-NAME"
    psk="NETWORK-PASSWORD"
}
```

Reference: https://desertbot.io/blog/headless-raspberry-pi-3-bplus-ssh-wifi-setup

### Kết nối dây Raspberry với RF module

| PIN	NRF24L01	| RPI	RPi -P1 Connector |
| ------------- | --------------------- |
| 1 GND	       | rpi-gnd	(25) |
| 2 VCC	      | rpi-3v3	(17) |
| 3 CE	       | rpi-gpio22	(15) |
| 4 CSN	       | rpi-gpio8	(24) |
| 5 SCK	       | rpi-sckl	(23) |
| 6 MOSI	     | rpi-mosi	(19) |
| 7 MISO	     | rpi-miso	(21) |
| 8 IRQ		     |  not connected  / optional |

Reference: https://pinout.xyz/pinout/spi

### Test Truyền nhận với Arduino

Nạp code RX_GettingStarted cho arduino.

Cài thư viện c cho NRF24L01 trên Raspberry Pi (https://tmrh20.github.io/RF24/Linux.html):

1. `wget http://tmrh20.github.io/RF24Installer/RPi/install.sh`
2. `chmod +x install.sh`
3. `./install.sh`
4. `cd rf24libs/RF24/examples_linux`
5. `nano gettingstarted.cpp`
6. `make`
7. `sudo ./gettingstarted`

![alt text](DOCS/test_rf_arduino_raspberry.png)

Reference: 

- https://tmrh20.github.io/RF24/Linux.html

- https://gist.github.com/natevw/5789019


### Bật tính năng SPI

Bỏ command dòng `dtparam=spi=on` trong file `/boot/config.txt` sau đó khởi động lại Raspberry Pi.

run `ls /dev/spi*` để kiểm tra.

### Cài nodejs

Step 1: Detect What Version of Node.js You Need

`uname -m`

Step 2: Download Node.JS Linux Binaries for ARM

`wget https://nodejs.org/dist/v12.18.0/node-v12.18.0-linux-armv7l.tar.xz`

Step 3: Extract the Archive

`tar -xz node-v12.18.0-linux-armv7l.tar.xz`

Step 4: Copy Node to /usr/local

`cd node-v12.18.0-linux-armv7l`

`sudo cp -R * /usr/local/`

Step 5: Check If Everything Is Installed Ok

 `node -v`

 `npm -v`

References:

- https://www.instructables.com/id/Install-Nodejs-and-Npm-on-Raspberry-Pi/

- https://medium.com/@bipul.k.kuri/upgrade-node-js-npm-in-raspbian-on-raspberry-pi-f1bcdaa23db5

- https://linuxize.com/post/how-to-install-node-js-on-raspberry-pi/

- install node-gyp: https://www.npmjs.com/package/node-gyp

### Cài đặt thư viện NRF24L01 trên Raspberry Pi

`npm install nrf24 --save`

`cd node_modules/nrf24/examples`

`sudo node GettingStarted.js`

References:

- https://github.com/ludiazv/node-nrf24