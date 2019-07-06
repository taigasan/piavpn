# piavpn
It is a vpn manager for Private Internet Access VPN. This program is inspired by the pia manager from d4rkcat.  
It is designed for Debian based distro, and is still on work.

Run this at your own risk.



Usage:
==========
After installation, to open a VPN tunnel to a PIA server, run:
	
	piavpn

To let pia select the closest server:

	# strong-tcp is the default protocol
	piavpn --auto

At first launch, you'll be prompted for pia user credentials. They will be saved in /etc/piavpn/credentials.d/credentials.

	usage: piavpn [-adhp] [--auto] [--protocol <prot>]
				  [--debug] [--help]

	options summary:
	-a  --auto      automatically connect to closest server
	-p  --protocol  select <prot> communication protocol.
					Choices are: tcp, udp, stcp, sudp 
	-d  --debug     high verbosity
	-h  --help      show this help message




Dependencies:
==========
Required:
- openvpn
- curl
- net-tools
- unzip
- openssl
- dnsutils

Suggested:
- dnsmasq

To manually install dependencies:  
	
	apt update  
	apt install openvpn curl unzip

you can remove then later with:  
	
	apt remove openvpn curl unzip

be sure to install the latest versions.



Installation:
==========
copy the repository to your disk:  
	
	git clone https://this.repo.url.git destination_dir


install, or not:  
	
	make install

uninstall:  
	
	# remove application directories as well
	make uninstall



Security and Privacy:
==========
Do mind:
- credentials are store in clear, in /etc/piavpn/credentials.d/.  
	This should change in the future.
- This program does not manage DNS, DNS leaks may occurs.  
	Please check that here:  
			https://ipleak.net  
			https://dnsleaktest.com  
	To prevent this, you could install dnsmasq, and use PIA DomainNameServer.  
	See Dnsmasq Section below.


Using Dnsmasq:
==========
Dnsmasq is an easy lightweight dns server.

installing dnsmasq:  
	
	apt update  
	apt install dnsmasq  

edit /etc/dnsmasq.conf, with those lines:
	
	# sanity options
	domain-needed
	bogus-priv
	# do not use resolv.conf to determine used NameServers
	no-resolv
	# set Nameservers to PIA's
	server=209.222.18.222
	server=209.222.18.218

restart dnsmasq 
	
	service dnsmasq restart

check it works:  

	# get ip where dns request are send  
	# this is usually 127.0.0.1:53 ( localhost, port 53)
	nslookup server

	# this show what program carries dns request  
	# it should return dnsmasq  
	netstat -lpnt | grep "127.0.0.1:53"
