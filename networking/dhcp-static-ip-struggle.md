                                                  March 02/2026
# DHCP & Static IP Configuration - The Struggle is Real!
- Today I learned about static IP address and DHCP. 
## 🎯 What I Learned
- Set up static IP on Ubuntu as well as CentOS clients
- Configured DHCP server on CentOS Stream 10 and Ubuntu (and discovered the hard way that `dhcp-server` package is removed by RHEL!)
- Finally got both VMs talking to each other

## 🔥 The Problems I Faced
1. **Package not found error** - Spent hours debugging why `dhcp-server` wouldn't install
2. **Service name confusion** - Kept trying `systemctl status dhcp` instead of `dhcpd`
3. **CentOS Stream 10 surprise** - Learned the hard way that traditional DHCP is gone
4. **Couln't directed to local.repo** - created local.repo repository instead of conf file

## 💡 How I Fixed It
1. **chose HOSTONLY ETHERNET ADAPTER** - insted of copiying repos, tried another way to set up DHCP. in VMBox->Network->HOSTONLY ETHERNET ADAPTER. Then ran nmtui-> selected Auto for IPv4.

## Commands Used
1. ip a
2. nmcli
3. nmtui
4. sudo ip link set <device_name> down
5. sudo ip link set <device_name> up
6. hostname -I
7. ping <IP add>
sudo nmcli connection down (FORCE DHCP REQUEST FOR 1 VM THAT WAS NOT GETTING DYNAMIC ID AUTOMATICALLY)
sudo nmcli connection up (FORCE DHCP REQUEST FOR 1 VM THAT WAS NOT GETTING DYNAMIC ID AUTOMATICALLY)

<img width="1280" height="800" alt="Static IP issue in Ubuntu" src="https://github.com/user-attachments/assets/29a51688-bf01-4c55-8e50-02a3381cc27d" />

<img width="1280" height="800" alt="Finally DHCPs up" src="https://github.com/user-attachments/assets/809ef649-4156-4bdd-bab9-0338667f130f" />
