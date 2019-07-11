Evil Access Point Defender (EAPD) is an application that helps wireless network admins discover and prevent bad access points from attacking wireless network users. It's a configurable intrusion detection/intervention system for wifi.

The application can be run in regular intervals to protect your wireless network from Evil Twin like attacks. By configuring the tool you can get notifications sent to your email whenever an evil access point is discovered. Additionally you can configure the tool to perform DoS on the legitimate wireless users to prevent them from connecting to the discovered evil AP in order to give the administrator more time to react. However, notice that the DoS will only be performed for evil APs which have the same SSID but different BSSID (AP’s MAC address) or running on a different channel. This is to avoid DoS your legitimate network.

The tool is able to discover Evil APs using one of the following characteristics:

Evil AP with a different BSSID address Evil AP with the same BSSID as the legitimate AP but a different attribute (including: channel, cipher, privacy protocol, and authentication) Evil AP with the same BSSID and attributes as the legitimate AP but different tagged parameter - mainly different OUI (tagged parameters are additional values sent along with the beacon frame. Currently no software based AP gives the ability to change these values. Generally software based APs are so poor in this area). Whenever an Evil AP is discovered the tool will alert the admin through email. Additionally the tool will enter into preventive mode in which the tool will DoS the users of the legitimate wireless network from connecting to the discovered Evil AP. The tool can be configured easily by starting in what we call “Learning Mode”. In this mode you can whitelist your legitimate network. This can be done by following the wizards during the Learning Mode. You can also configure the preventive mode and admin notification from there as well.

This release comes with an installer and support for all OpenWrt 15.x/^ Devices, ie. the Wifi-Pineapple Tetra. A module will soon be written to add easy access on the Wifi-Pineapple PineAP Management Interface.

From a clean install just issue these following commands to run the installer:

     cd

     opkg update && opkg install git-http

     git clone https://github.com/0mniteck/EAPD

     cd EAPD

     chmod 744 INSTALL.sh && chmod +x INSTALL.sh 

     ./INSTALL.sh

If INSTALL.sh fails for some reason or you need to add an external extroot then you need to manualy install using this guide.

Requirements:

Openwrt 15.x and above (Recommend running it on a Wifi-Pineapple Tetra with firmware 2.5.4)

30MB Minimum Free Space Required (50MB Minimum Free Space Recommended)

     If you have a low memory device like the Wifi-Pineapple Nano then you need to add external storage,
     and use extroot to have enough space for a full python installation.
     Check the following URL: https://openwrt.org/docs/guide-user/additional-software/extroot_configuration
     But even with 'nice -n 19 ash' you may have stability issues with an external extroot.

Aircrack-ng and Airmon-ng

     Your wireless card must be supported by Aircrack-ng.
     Check the following URL: http://www.aircrack-ng.org/doku.php?id=compatibility_drivers#which_is_the_best_card_to_buy

MySQL-Server and Procps-ng-pkill

     If you are using 18.x and up also install mariadb-client mariadb-server-plugin-auth-socket
     edit /etc/init.d/mysqld by adding --skip-grant-tables,
     because the mysqld on openwrt doesn't have a workable way to run mysql_secure_install,
     run mkdir /mnt/data/ && mkdir /mnt/data/mysql/ then run mysql_install_db --force

Python, Python-PiP, and Python-MySQL

     Install Python libraries: NetAddr and Scapy by running 'pip install netaddr scapy'
     On a reinstall pip will fail with an error code,
     here is the fix: https://stackoverflow.com/a/49900741

Git and Git-http

     Clone Git

     Stop Cron /etc/init.d/cron stop

     Move CRONTABS to /etc/crontabs/root

     Move MYSQLD and EAPDD to /etc/init.d/eapdd

     Move EAPD.py to /root/eapd.py

     Then run chmod 744 /etc/init.d/eapdd && chmod +x /etc/init.d/eapdd

     And run chmod 744 /etc/init.d/mysqld && chmod +x /etc/init.d/mysqld

     Make sure its disabled by running /etc/init.d/eapdd disable along with /etc/init.d/mysqld disable

Learning Mode: This Mode can be invoked with the “L” switch. When running the tool in this mode the tool will start by scanning for the available wireless networks. Then it lists all the found wireless networks with whitelisted APs colored with green. It also lists the whitelist APs and OUIs (tagged parameters). The tool also provides several options which allow you to add/remove SSIDs into/from whitelist. You need to whitelist your SSID first before running the tool in the Normal Mode. Moreover, you can configure Preventive Mode from “Update options -> Configure Preventive Mode”. First you need to set the Deauthentication time (in seconds) into a number bigger than 0 (setting the value to 0 will disable this mode). Then you need to set the number of time to repeat the attack. This is so important for attacking more than Evil AP because the tool cannot attack all of them in the same time (how can you attack several APs on different channels? Later on we will improve the tool and allow it to attack (in the same time) several APs in the same channel). The tool will attack the first Evil AP for specified deauthentication time then it will stop and attack the second one and so on. Be careful from increasing the Deatuth time so much because this may attack only one AP and leaving the others running. My recommendation is to set the Deauth time to something suitable such as 10 seconds and increasing the repeat time. Finally, you can configure admin notification by setting admin email, SMPT server address, SMTP username (complete email address) for authentication purpose, and SMTP password. You can use any account on Gmail or your internal SMTP server account.

     Run Learning Mode before starting '/etc/init.d/cron' by issuing '/etc/init.d/eapdd L'

Scanning Mode: This is the mode in which the tool starts to discover Evil APs and notify the administrator whenever one is discovered.

     This mode can be invoked with '/etc/init.d/eapdd start'
     or schedule regular runs by invoking '/etc/init.d/cron enable && reboot'

Just issue these following commands to run the uninstaller:

     cd

     cd EAPD

     chmod 744 UNINSTALL.sh && chmod +x UNINSTALL.sh 

     ./UNINSTALL.sh