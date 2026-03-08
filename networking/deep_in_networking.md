📅 FEB 26, 2026
IP Address
Internet Protocol address - A unique identifier for computer devices on a network.

Port Numbers
Port numbers are logical addresses for applications or processes that use network/internet communication. They're assigned automatically by the OS or manually by users.

Port numbers are used in TCP/UDP-based networks, with a range of 65,536 available port numbers.

Common Ports:

Port	Protocol	Service
80	TCP	HTTP
443	TCP	HTTPS
21	TCP	FTP
22	TCP	SSH
53	TCP/UDP	DNS
25	TCP	SMTP (Email)
Protocols
When computers communicate, they establish a session and agree on transmission rules:

Transmission speed

Data file size

Security measures

Flow control

These rules are called protocols and are defined in networking models.

OSI Model (7 Layers):
Physical

Data

Network

Transport

Session

Presentation

Application

TCP/IP Model (4 Layers):
Network Interface

Internet

Transport

Application


📅 FEB 27, 2026
IPv4 Addressing
32 bits total (4 octets × 8 bits)

IPv4 Classes
Class	Network/Host Bits	First Bit Pattern	Subnet Mask	Prefix	Address Range
Class A	N=8 bits, H=24 bits	0	255.0.0.0	/8	0-127
Class B	N=16 bits, H=16 bits	10	255.255.0.0	/16	128-191
Class C	N=24 bits, H=8 bits	110	255.255.255.0	/24	192-223
Class D	Multicast	-	-	-	224-239
Class E	R&D Purpose	-	-	-	240-255
Networking Concepts
Subnet - A logical portion of a larger network

Subnetting - Sub-dividing a large network into smaller networks

Subnet Mask - 32-bit address describing network and host portions

Prefix Length - Total number of network bits (Netmask)

CIDR - Classless Inter-Domain Routing

Network Commands
bash
ip a                          # Check interface status
nmcli                         # Network Manager CLI
sudo ip link set <device> down  # Bring interface down
sudo ip link set <device> up    # Bring interface up
nmtui                         # Configure IP manually (Text UI)
hostnamectl                   # Check hostname
hostname -I                   # Check IP address
ping <IP>                     # Test connectivity


📅 FEB 28, 2026
DHCP - Dynamic Host Configuration Protocol
DHCP is a network protocol that enables a server to automatically assign IP addresses and provide network configuration parameters to clients from a predefined IP pool.



📅 MAR 02, 2026
SSH - Secure Shell Protocol
Purpose: Manage remote Linux/Windows servers

SSH - For Linux systems

RDP - Remote Desktop Protocol (for Windows)

SSH Details
Package: openssh-server

Protocol: SSH

Port: 22

Service name: sshd

Config file: /etc/ssh/sshd_config

Log file: /var/log/secure

Installation Commands
RHEL-based:

bash
yum/dnf install openssh-server -y
systemctl start sshd
systemctl enable sshd
systemctl status sshd
Ubuntu/Debian:

bash
sudo apt-get install openssh-server -y
sudo systemctl start sshd
sudo systemctl enable sshd
sudo systemctl status sshd
Access a Remote Server
bash
ssh <options> username@IP_address
Network Adapter Configuration
Bridge Adapter - Used to SSH to a device on another network

# Set Bridge adapter (enp0s9)
nmcli device connect enp0s9


📅 MAR 04, 2026
SSH Authentication Methods
Password-based (default)

Key-passwordless

Key-passphrase

Allow Root Login via SSH
Edit /etc/ssh/sshd_config and set:

text
PermitRootLogin yes
Generate SSH Key Pair
bash
ssh-keygen
This creates:

Private key: id_rsa

Public key: id_rsa.pub

Key-Passwordless Authentication
On client machine:

bash
ssh-keygen              # Leave empty for no password
ssh-copy-id <username>@<server_IP>
Key-Passphrase Authentication
On client machine:

bash
ssh-keygen              # Enter desired passphrase
ssh-copy-id <username>@<server_IP>
Important: Always run systemctl restart sshd after modifying /etc/ssh/sshd_config

Check Service Ports
bash
ss -plant | grep <service_name>
netstat -plant | grep <service_name>


📅 MAR 05, 2026
File Transfer Between Servers
SCP (Secure Copy)
Works over SSH protocol

bash
scp <options> user@remote1:/source/path user@remote2:/destination/path
RSYNC (Remote Sync)
Works over remote-update protocol

bash
rsync <options> user@remote1:/source/path user@remote2:/destination/path
WinSCP
Application for Windows ↔ Linux file transfers

SFTP (SSH File Transfer Protocol)
Used to transfer files between Linux and/or Windows servers. Configure via /etc/ssh/sshd_config.

Restrict User to SFTP Only (No Shell Access)
bash
# Create directory
mkdir /myapps

# Create user with nologin shell and home directory
useradd -Md /myapps -s /sbin/nologin username
Note: Always run systemctl restart sshd after configuration changes

📅 MAR 06, 2026
FTP - File Transfer Protocol
Package: vsftpd

📌 Quick Reference
Command	Purpose
ip a	Show IP addresses
nmtui	Configure network manually
systemctl restart sshd	Apply SSH changes
ssh-keygen	Generate SSH keys
ssh-copy-id user@IP	Copy SSH keys to server
scp	Secure file copy
rsync	Remote file sync
This document represents my ongoing networking learning journey. Last updated: March 2026



