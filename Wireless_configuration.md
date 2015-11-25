# WIfi_Configuration_RaspberryPi
Security System Wireless Configuration

# Setting Wifi Up Via Command Line

To scan for WiFi networks, use the command "sudo iwlist wlan0 scan"

Open the wpa-supplicant configuration file in nano: 

note: you must check the file and open the file configuration

    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1

    network={
        ssid="The_ESSID_from_earlier"
        psk="Your_wifi_password"
        key_mgmt=WPA-PSK
    }

example network :

    network={
        ssid="testing"
        psk="testingPassword"
    }

Now save the file by pressing ctrl+x then y, then finally press enter.

    sudo ifdown wlan0 and sudo ifup wlan0, or reboot your Raspberry Pi with sudo reboot.

note: if you go to reboot system and then you must check your access internet, maybe ping google.com or ping detik.com or else

On the Raspberry Pi (and on Linux in general) you configure your network settings in the file “/etc/network/interfaces”. 

    sudo nano /etc/network/interfaces

To configure you wireless network you want to modify the file such that it looks like the following:

    auto lo
    iface lo inet loopback
    iface eth0 inet dhcp

    allow-hotplug wlan0
    auto wlan0

    iface wlan0 inet dhcp
    wpa-ssid "Your Network SSID"
    wpa-psk "Your Password"
   
note: if you make your network SSID and make your passwaord check your wireless to access the system

To save the file press Ctrl+O and to save and exit Ctrl+X and press enter

check the configuration wireless  : sudo service networking reload

    ifconfig wlan0

    wlan0 Link encap:Ethernet HWaddr 80:1f:02:aa:12:58
          inet addr:192.168.1.8 Bcast:192.168.1.255 Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
          RX packets:154 errors:0 dropped:173 overruns:0 frame:0
          TX packets:65 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:32399 (31.6 KiB) TX bytes:13036 (12.7 KiB)
      
note: if youre access to internet the connection see Tx and Rx 

Referrence : http://raspberrypihq.com/how-to-add-wifi-to-the-raspberry-pi/  https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md  https://www.maketecheasier.com/setup-wifi-on-raspberry-pi/



