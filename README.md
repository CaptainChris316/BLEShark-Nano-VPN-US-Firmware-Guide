# BLEShark-Nano-VPN-US-Firmware-Guide
I wrote down a guide for EU customers of the BLEShark Nano to download the US firmware instead of the EU firmware. 
The reason why i have to split it between my windows pc and my kali linux notebook is that my windows pc don't have a inbuild wifi module on the mainboard.
My external wifi adapter is not working correctly as AP at windows. So i have to do this with this workaroud. 
The guide is structurally not perfect yet, so there will be an update as soon as I have more time for it.


## The Guide

  1. I setup a vpn connection over wireguard. Download and install wireguard for windows on your windows pc.
  2. Go to your protonvpn site/ dashboard an download a US-Free server config file
     <img width="944" height="506" alt="image" src="https://github.com/user-attachments/assets/946801d5-2902-45b8-bc22-be89f99e0b39" />
     <img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/9cbdca62-9141-4aa9-ae2e-60c81e8384c3" />
     <img width="596" height="469" alt="image" src="https://github.com/user-attachments/assets/b341ab54-a737-45b1-81f5-7ff1371159b7" />
  
     import the config file in to WireGuard and activate the connection.
  3. open with chrome browser the flash web page of the BLEShark nano [BLEShark nano flasher](https://flasher.infishark.com/)
  4. after flashing switch to your kali linux device and set , in my case the external alfa network Usb WLAN – adapter as hotspot. My external USB adapter Model ist the **Alfa (AWUS036ACM)**.
  5. open firefox on your kali linux device and open the protonvpn page / dashboard, go to downloads for a free vpn US-Free, but this time scroll further down to the Openvpn section.
     <img width="945" height="569" alt="image" src="https://github.com/user-attachments/assets/e4c01378-b588-4eb4-9c49-41e33cb6c8fb" />
     <img width="945" height="611" alt="image" src="https://github.com/user-attachments/assets/36bfb571-1b5c-4e6c-ba0d-4f917e176c96" />
     <img width="945" height="766" alt="image" src="https://github.com/user-attachments/assets/e188421a-bc47-4bec-be07-f7d8e7d2c3fc" />
     Let the tab in your browser with your OpenVPN account name open, you will need it to copy the login data.
  6. open the terminal in kali linux  and have a look if your wifi adapter supports *AP
     ```bash
     iw list | grep -A 10 “Supported interface modes“ 
     ```
     you need to have the *AP as note
     ```markdown
     * AP
     ```
     Next step is to look whats the name of your wifi network adapter
     ```bash
     ip a
     ```
     it will list your devices, usually your inbuild wifi adapter is named like **wlan0** by default if you use an external adapter too it continues **wlan1** etc.
     now it comes to the main part for starting the hotspot, here is the command:
     ```bash
     nmcli device wifi hotspot ifname wlan0 ssid KaliHotspot password 12345678
     ```
     created 
     - SSID: KaliHotspot
     - WPA2-Passwort: 12345678 
     - DHCP automatic

  7. Now whe have to create the vpn connection on the kali linux device. Go to network settings and create a vpn connection
     <img width="945" height="692" alt="image" src="https://github.com/user-attachments/assets/aaa2212e-aa34-470b-a3b0-59722910b3f2" />
     <img width="945" height="594" alt="image" src="https://github.com/user-attachments/assets/60ac92bd-9405-4f64-b177-ab04820a645d" />
     <img width="945" height="687" alt="image" src="https://github.com/user-attachments/assets/1e086c14-d7ba-4bba-9478-bf8dc88bb49f" />
     You will be asked how you will setup the VPN connection, you have to choose you like to import a saved vpn configuration file.
     <img width="944" height="197" alt="image" src="https://github.com/user-attachments/assets/cfa7c5d6-0165-4f1b-865b-eaf2c55e6ee2" />
  8. After import now you have to jump back to point 5 where you can copy your login date to the login fields of the vpn connection in settings which we setup now.
  9. Luckily when you setup vpn right and you activate the vpn connection it will be look like this: 
     <img width="470" height="204" alt="image" src="https://github.com/user-attachments/assets/0d4fce29-a45b-4a47-8e67-88e644d4c14a" />

  10.	You are now connected with the US-Free Server of ProtonVPN, you can make a test and open [What is my ip address](https://www.whatismyipaddress.com) to see where your ip address is coming from. The ip address should be registered in
      USA somewhere now. 
  12.	Finaly boot your BLEShark nano and connet with your mobile phone to BLEShark Setup, let it scan your wifi and choose ( in my case **KaliHotspot** ) and login with your given password. The BLEShark will say that he is now connected with **KaliHotspot**
      succesfully. You can watch, after the restart, at the BLEShark nano that he is try to requesting the signed url and it start to download over the hotspot to the BLEShark nano.

     	To deactivate the hotspot just type:
     	
      nmcli connection down Hotspot
      
