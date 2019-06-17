

`RASPBIAN`
=======
***TL;DR***: just stuff and testing markdown.Maybe even usefull infos in here 


<details><summary><b>Andrewj freyer's Monitor.sh </b></summary>

https://github.com/andrewjfreyer/monitor/



after installing 
`sudo nano /etc/systemd/system/monitor.service` and in the 7th line 


`ExecStart=/bin/bash /home/pi/monitor/monitor.sh -b -x  & `

` -b`  for Beacon support and ` -x` for retained MQTT messages



https://github.com/andrewjfreyer/monitor/#setup-monitor



</details>


<details><summary><b>Billw2's rpi-clone </b></summary>


 On the Raspberry Pi:
```
	$ git clone https://github.com/billw2/rpi-clone.git 
	$ cd rpi-clone
	$ sudo cp rpi-clone rpi-clone-setup /usr/local/sbin
	$ lsblk
	$ sudo rpi-clone sdX
  
  ```
  (fyi: lsblk stands for list all block devices)

</details>


<br>

<details><summary><b>Raspberry Pi Wireless Set Up</b></summary>


# Installation Instructions for Raspberry Pi Zero W

## SD Card

1. Download latest version of **raspbian** [here](https://downloads.raspberrypi.org/raspbian_lite_latest)

2. Download for example etcher from [etcher.io](https://etcher.io)

3. Image **raspbian lite stretch** to SD card. [Instructions here.](https://www.raspberrypi.org/magpi/pi-sd-etcher/)

4. Mount **boot** partition of imaged SD card (unplug it and plug it back in)

5. **To enable ssh,** create blank file, without any extension, in the root directory called **ssh**

6. **To setup Wi-Fi**, create **wpa_supplicant.conf** file in root directory and add Wi-Fi details for home Wi-Fi:

```bash
country=US
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1

network={
    ssid="Your Network Name"
    psk="Your Network Password"
    key_mgmt=WPA-PSK
}
```



</details>
