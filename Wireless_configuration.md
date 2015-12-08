# WIfi_Configuration_RaspberryPi
Security System Wireless Configuration

# Setting Wifi Up Via Command Line

To scan for WiFi networks, use the command "sudo iwlist wlan0 scan"

Open the wpa-supplicant configuration file in nano: 

note: you must check the file and open the file configuration

    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1
    ap_scan=1
    eapol_version=1
    fast_reauth=1


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
    iface wlan0 inet manual
    wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
    iface default inet dhcp
   
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

Install Server

    sudo apt-get update
    sudo apt-get install hostapd isc-dhcp-server
    
Set up DHCP server

    sudo nano /etc/dhcp/dhcpd.conf
    
Find the line that say :

    option domain-name "example.org";
    option domain-name-servers ns1.example.org, ns2.example.org;

And change to them to add a # in the beginning to say :

    #option domain-name "example.org";
    #option domain-name-servers ns1.example.org, ns2.example.org;

Find the line that say :

    # If this DHCP server is the official DHCP server for the local
    # network, the authoritative directive should be uncommented.
    #authoritative;

and remove the # so it says

    # If this DHCP server is the official DHCP server for the local
    # network, the authoritative directive should be uncommented.
    authoritative;

running the file : /etc/default/isc-dhcp-server

    sudo nano /etc/default/isc-dhcp-server
    
and scroll down to INTERFACES="" and update it to say INTERFACES="wlan0" 
close and save the file CTRL X and then Y enter

Configure Access Point

Now we can configure the access point. We will set up a password-protected network so only people with the password can connect

    sudo nano /etc/hostapd/hostapd.conf
    
Paste the following in, you can change the text after ssid= to another name

    interface=wlan0
    driver=rtl871xdrv
    ssid=Pi_AP
    hw_mode=g
    channel=6
    macaddr_acl=0
    auth_algs=1
    ignore_broadcast_ssid=0
    wpa=2
    wpa_passphrase=Raspberry
    wpa_key_mgmt=WPA-PSK
    wpa_pairwise=TKIP
    rsn_pairwise=CCMP
    
Now we will tell the Pi where to find this configuration file. Run

    sudo nano /etc/default/hostapd
    
Find the line #DAEMON_CONF="" and edit it so it says : DAEMON_CONF="/etc/hostapd/hostapd.conf"
Don't forget to remove the # in front to activate it!
then save the file CTRL X then Y and enter

Referrence : http://raspberrypihq.com/how-to-add-wifi-to-the-raspberry-pi/  https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md  https://www.maketecheasier.com/setup-wifi-on-raspberry-pi/
https://learn.adafruit.com/setting-up-a-raspberry-pi-as-a-wifi-access-point/install-software


