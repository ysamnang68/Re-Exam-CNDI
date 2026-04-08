Lab1: Basic Configure
🔧 1. Set Router Identity
/system identity set name="YourName"
________________________________________
🌐 2. Configure WAN (ether1 → NAT/Internet DHCP)
/ip dhcp-client add interface=ether1 disabled=no
________________________________________
🖥️ 3. Configure LAN (ether2 → PC1)
/ip address add address=192.168.10.1/24 interface=ether2
________________________________________
📡 4. Enable NAT (Internet Access)
/ip firewall nat add chain=srcnat out-interface=ether1 action=masquerade
________________________________________
📥 5. Setup DHCP Server for PC
/ip pool add name=dhcp_pool ranges=192.168.10.10-192.168.10.100

/ip dhcp-server add name=dhcp1 interface=ether2 address-pool=dhcp_pool disabled=no

/ip dhcp-server network add address=192.168.10.0/24 gateway=192.168.10.1 dns-server=8.8.8.8


👤 6. Create Users
/user add name="your_full_name" password="123456" group=full
/user add name="ITAdmin" password="123456" group=write
/user add name="Support" password="123456" group=full
________________________________________
🔐 7. Allow Only Winbox & Disable Others
/ip service disable telnet
/ip service disable ftp
/ip service disable www
/ip service disable ssh
/ip service disable api
/ip service disable api-ssl
________________________________________
🔌 8. Change Winbox Port to 900
/ip service set winbox port=900
________________________________________
🚫 9. Disable MAC Access (No MAC Winbox)
/tool mac-server set allowed-interface-list=none
/tool mac-server mac-winbox set allowed-interface-list=none

# 🚫 10. Disable Neighbor Discovery (Scan Router)
/ip neighbor discovery-settings set discover-interface-list=none
________________________________________
💾 11. Backup Configuration
/system backup save name=mybackup
/export file=myconfig
________________________________________
🔄 12. Reset Router
/system reset-configuration
________________________________________
♻️ 13. Restore Backup
/system backup load name=mybackup.backup
