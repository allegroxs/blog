I tried PPTP on LuCI and successfully connect to it with my phone(data roaming), but others can not. 

It is said that only via local net can we visit this vpn. So I changed the firewall configuration(indiscriminately by myself...), then my network suddenly shut down... And the wan light flashed red... Super sorry at that time but fortunately it returned normal after nearly 15 minutes.

Maybe I'll try openVPN in the furure.Official tutorial reference:
https://openwrt.org/zh/docs/guide-user/services/vpn/openvpn/basic


## If you want to use DDNS
I recommend [YDNS](ydns.io), cause 
- It is free to register and create many host
- Usable for mainland China
- Easy to create the request url of updating ip addr of the host. If you click the url, current ip will be set for the dynamic domain name . You can create a cron task on the OpenWrt linux to send the request.