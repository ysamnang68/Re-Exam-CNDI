Lab2: Configure DHCP
🔧 1. Set Identity
/system identity set name="YourName"
________________________________________
🔄 2. Rename Interfaces
/interface ethernet
set ether1 name=WAN
set ether2 name=LAN-IT
set ether3 name=LAN-HR
set ether4 name=LAN-Customer
set ether5 name=LAN-Sale
________________________________________
🌐 3. Assign IP Address to Each LAN
/ip address
add address=192.168.10.1/24 interface=LAN-IT
add address=192.168.20.1/24 interface=LAN-HR
add address=192.168.30.1/23 interface=LAN-Customer
add address=192.168.40.1/24 interface=LAN-Sale
✅ Customer uses /23 because 300 devices need larger subnet
________________________________________
📡 4. WAN DHCP Client
/ip dhcp-client add interface=WAN disabled=no
________________________________________


📥 5. Create DHCP Pools
/ip pool
add name=pool-IT ranges=192.168.10.20-192.168.10.49
add name=pool-HR ranges=192.168.20.10-192.168.20.19
add name=pool-Customer ranges=192.168.30.10-192.168.31.254
add name=pool-Sale ranges=192.168.40.30-192.168.40.129
________________________________________
⚙️ 6. Create DHCP Server
/ip dhcp-server
add name=dhcp-IT interface=LAN-IT address-pool=pool-IT lease-time=8h disabled=no
add name=dhcp-HR interface=LAN-HR address-pool=pool-HR lease-time=8h disabled=no
add name=dhcp-Customer interface=LAN-Customer address-pool=pool-Customer lease-time=8h disabled=no
add name=dhcp-Sale interface=LAN-Sale address-pool=pool-Sale lease-time=8h disabled=no
________________________________________
🌍 7. DHCP Network (Gateway + DNS)
/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=8.8.8.8
add address=192.168.20.0/24 gateway=192.168.20.1 dns-server=8.8.8.8
add address=192.168.30.0/23 gateway=192.168.30.1 dns-server=8.8.8.8
add address=192.168.40.0/24 gateway=192.168.40.1 dns-server=8.8.8.8
________________________________________
🔥 8. NAT (Internet Access)
/ip firewall nat add chain=srcnat out-interface=WAN action=masquerade
________________________________________
📌 9. Static IP for Specific PCs
/ip dhcp-server lease
add address=192.168.10.5 mac-address=XX:XX:XX:XX:XX:XX comment="PC1-IT"
add address=192.168.40.10 mac-address=YY:YY:YY:YY:YY:YY comment="PC4-Sale"
________________________________________
🔒 10. ARP Reply-Only (LAN Customer Security)
/interface ethernet set LAN-Customer arp=reply-only
👉 Only devices with DHCP lease/static ARP can communicate

