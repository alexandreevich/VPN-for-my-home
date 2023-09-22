# My personal VPN
This is my checklist for creating a personal VPN service. I used these commands and the service is still working.
As host VM I'm use racknerd . com  [MyReferalLink](https://my.racknerd.com/aff.php?aff=8305)
1 Gb KVM VPS with 1 vCPU Core. I could have used less power, but I took it with a reserve. It may be needed for something else. 
Let's go!


---
## 1. Commands and comments
### Authorization 

1. **ssh-keygen** — At first, generate ssh-key pair on local pc ( *ssh-keygen -t rsa -C "your_email@example.com", then /home/.ssh/id_newname* 
   

1. **ssh root@123.123.123.123**  — connecting via the terminal 

1. **ssh-copy-id root@123.123.123.123** -  copy my ssh-key for authorization in the future

1. **sudo nano /etc/ssh/sshd_config**  - change our values for safety
  * Port *random*
  * PasswordAuthentication no
  * sudo service ssh restart

1. **apt update && apt upgrade -y**  - as a habit

---
## 2. Safety 
### An important part for safety and internet hygiene

1. **apt install ufw**  - firewall who cover your VM. 

2. **nano /etc/ufw/before.rules** (*be carefull, he will close all port after saving settings*) 
* ~~#"-A ufw-before-input -p icmp —icmp-type echo-request -j ACCEPT"~~  - comment this line
* **A ufw-before-input -p icmp —icmp-type echo-request -j DROP** - add this line
*Really, I dont know why, but it is important for the succesfull pass the Tunnel Detection test (two-way ping).
3. **Open this port's**
* ufw allow 80
* ufw allow 443
* ufw allow  - your SSH port from 1.4 
* ufw allow 54321 - random port for wireguard 
* ufw allow 1194 - port for OpenVPN. I need this to create VPN client on my router.
4. ufw disable && ufw enable** - Finally, restart firewall. 

### Autoupdate safety
**apt-get install unattended-upgrades**
**nano /etc/apt/apt.conf.d/20auto-upgrades:**
* APT::Periodic::Update-Package-Lists "1";
* APT::Periodic::Unattended-Upgrade "1";
* APT::Periodic::Download-Upgradeable-Packages "1";
* APT::Periodic::AutocleanInterval "7";

---
## 3. For mobile
### I choise Wireguard. Easy & fastl.
**apt install -y curl**
**curl -O [https://github.com/angristan/wireguard-install](https://raw.githubusercontent.com/angristan/wireguard-install/master/wireguard-install.sh)**
**chmod +x wireguard-install.sh** 

*Do not forget about the open port from point 2.3, when you will run your app. 
**./wireguard-install.sh** 
*For each new user, run the script again.*
Download wireguard to your device, then run the skript. He will give you a QR code. Scan and enjoy.

---

## 4. For router
### My router support openVPN, lets configurate him. 
* **wget https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh**
* **chmod +x openvpn-install.sh**
* **./openvpn-install.sh**

*Then copy on your PC file and add to router. A I used the program File2Zilla.* 

---

## Congratulations. You're great.


