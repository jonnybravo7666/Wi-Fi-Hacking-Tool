# Wi-Fi-Hacking-Tool

Hey there, In this blog we are going to see how to collect Wi-Fi information without using any tools or by installing any tool, we will do it just by simply running python code. The code only work when your divice is connected to Wi-Fi not with Ethernet cable (LAN)  

NOTE : Do not use this knowledge for any harmful purpose and do not get in trouble 

I divided it in three simple part just to know how this code work if you are beginner go with simple, then medium and then go for high. It will help you to collect lots of information. 

So let's get start 


# Simple code :

Python script that collect the information about Wi-Fi 

 To collect information about WiFi networks using Python, you can use the wifi library. 
 This library provides a set of functions to scan for available WiFi networks, get information about the current WiFi network, and connect to a WiFi network.
Here's an example Python script that uses the wifi library to scan for available WiFi networks and print their SSID (network name) and signal strength:

      Python code

      import wifi

      # scan for available WiFi networks
      networks = wifi.scan()

      # print the SSID and signal strength of each network
      for net in networks:
          print(f"SSID: {net.ssid}, Signal Strength: {net.signal}")



In this script, the wifi.scan() function is used to scan for available WiFi networks. This function returns a list of wifi.WiFiNetwork objects, each of which represents a single WiFi network. 
The ssid and signal attributes of each WiFiNetwork object are used to print the SSID (network name) and signal strength of each network.

Note that to use the wifi library, you need to install it first by running the command 'pip install wifi' in your terminal. Additionally, some functionality may be platform-specific, so the library may not work on all operating systems.


Python script that collect the information about Wi-Fi network 

To collect information about the current WiFi network that your device is connected to using Python, you can use the wlanapi module in the pywin32 library.
Here's an example Python script that uses the wlanapi module to get information about the current WiFi network and print its SSID, BSSID (access point MAC address), and signal quality:

      Python code

      import win32wifi

      # get information about the current WiFi network
      current_network = win32wifi.WlanAdapter().get_current_network()

      # get information about the current WiFi network
      current_network = win32wifi.WlanAdapter().get_current_network()

      # print the SSID, BSSID, and signal quality of the current network
      print(f"SSID: {current_network.ssid}")
      print(f"BSSID: {current_network.bssid}")
      print(f"Signal Quality: {current_network.signal_quality}%")


function is used to get information about the current WiFi network. This function returns a win32wifi.WlanNetwork object, which represents the current network. The ssid, bssid, and signal_quality attributes of the WlanNetwork object are used to print the SSID, BSSID (access point MAC address), and signal quality of the current network.

Note that to use the win32wifi module, you need to install the pywin32 library first by running the command pip install pywin32 in your terminal. Additionally, this module is only available on Windows operating systems.



# Medium code : 

Python script that collect the information about target Wi-Fi network 

 To collect information about a specific WiFi network using Python, you can use the wifi library. This library provides a set of functions to scan for available WiFi networks and get information about specific WiFi networks.
 Here's an example Python script that uses the wifi library to get information about a specific WiFi network and print its SSID, signal strength, and security type:

      Python code

      import wifi

      # specify the SSID of the target WiFi network
      target_ssid = "MyWiFiNetwork"

      # scan for available WiFi networks
      networks = wifi.scan()

      # find the target network and get its information
      target_network = None
      for net in networks:
          if net.ssid == target_ssid:
              target_network = net
              break

      # print the SSID, signal strength, and security type of the target network
      if target_network:
          print(f"SSID: {target_network.ssid}")
          print(f"Signal Strength: {target_network.signal}")
          print(f"Security Type: {target_network.security}")
      else:
          print(f"Could not find network with SSID {target_ssid}")


 In this script, the target_ssid variable is used to specify the SSID of the target WiFi network. The wifi.scan() function is used to scan for available WiFi networks, and the for loop is used to find the network with the specified SSID. The ssid, signal, and security attributes of the WiFiNetwork object representing the target network are used to print its SSID, signal strength, and security type.

Note that the security type is represented as a string, which may be one of the following values: 'open', 'wep', 'wpa', 'wpa2', 'wpa_wpa2'.



# High code 

Python script that collect the information about Wi-Fi network and device connected to it

To collect information about both the WiFi network and devices connected to it using Python, you can use the scapy library. This library provides a set of functions for sniffing network traffic and analyzing network packets.

Here's an example Python script that uses the scapy library to sniff WiFi traffic, get information about the WiFi network and devices connected to it, and print their SSIDs, MAC addresses, and IP addresses:

      Python code

      from scapy.all import *

      # specify the SSID of the target WiFi network
      target_ssid = "MyWiFiNetwork"

      # create a set to store the MAC addresses of connected devices
      connected_devices = set()

      # define a function to handle WiFi packets
      def handle_packet(packet):
          if packet.haslayer(Dot11Beacon):
              # check if the packet contains information about the target network
              ssid = packet[Dot11Elt].info.decode()
              if ssid == target_ssid:
                  # get the MAC address and signal strength of the access point
                  bssid = packet[Dot11].addr3
                  signal_strength = packet.dBm_AntSignal

                  # print the SSID, BSSID, and signal strength of the target network
                  print(f"SSID: {ssid}")
                  print(f"BSSID: {bssid}")
                  print(f"Signal Strength: {signal_strength} dBm")

          elif packet.haslayer(Dot11ProbeResp) and packet[Dot11].addr2 not in connected_devices:
              # check if the packet is a probe response from a connected device
              # and if the device's MAC address is not already in the set
              connected_devices.add(packet[Dot11].addr2)

              # get the MAC address and IP address of the connected device
              mac_address = packet[Dot11].addr2
              ip_address = packet[IP].src

              # print the MAC address and IP address of the connected device
              print(f"Device MAC Address: {mac_address}")
              print(f"Device IP Address: {ip_address}")

      # start sniffing WiFi traffic on the specified interface
      sniff(iface="wlan0mon", prn=handle_packet)


In this script, the target_ssid variable is used to specify the SSID of the target WiFi network. The connected_devices set is used to store the MAC addresses of devices that are connected to the target network.

The handle_packet function is defined to handle WiFi packets. If a packet contains information about the target network (i.e., it is a beacon frame with the specified SSID), the function prints the SSID, BSSID (access point MAC address), and signal strength of the network.

If a packet is a probe response from a connected device, and the device's MAC address is not already in the connected_devices set, the function adds the MAC address to the set and prints the MAC address and IP address of the device.

Finally, the sniff function is used to start sniffing WiFi traffic on the specified interface (wlan0mon in this example). The prn argument is used to specify the function that should be called for each packet that is sniffed.

Note that to use the scapy library, you need to install it first by running the command pip install scapy in your terminal. Additionally, some functionality may require root privileges or additional 
