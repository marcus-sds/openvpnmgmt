### iptable
![iptabl](https://www.frozentux.net/iptables-tutorial/images/tables_traverse.jpg =600)

### ssl negotiation
![ssl](https://tender.eprocurement.gov.in/DigitalCertificate/faqs/images/SSL.jpg)

### sslvpn
![sslvpn](https://community.jisc.ac.uk/system/files/images/tg-vpn-17.jpg)

### how to install openvpn
- os
- install openvpn
- cert,dh file creation
- network forwarding
>net.ipv4.ip_forward = 1
- nat or not

### vpn gateway creation
- install openvpn
- config and directory creation
- two-factor authentication

- vpn client log
> Wed Mar 15 08:23:48 2017 TLS: Initial packet from [AF_INET]
Wed Mar 15 08:23:48 2017 VERIFY OK: depth=1, C=US, ST=CA, L=SanFrancisco, O=Fort-Funston, OU=changeme, CN=changeme, name=changeme, emailAddress=mail@host.domain
Wed Mar 15 08:23:48 2017 VERIFY OK: nsCertType=SERVER
Wed Mar 15 08:23:48 2017 VERIFY OK: depth=0, C=US, ST=CA, L=SanFrancisco, O=Fort-Funston, OU=changeme, CN=changeme, name=changeme, emailAddress=mail@host.domain
Wed Mar 15 08:23:49 2017 Data Channel Encrypt: Cipher 'AES-256-CBC' initialized with 256 bit key
Wed Mar 15 08:23:49 2017 Control Channel: TLSv1.2, cipher TLSv1/SSLv3 DHE-RSA-AES256-GCM-SHA384

### p2p vpn creation
- install openvpn
- config creation
- key creation
> openvpn --genkey --secret /etc/openvpn/static.key

https://en.wikipedia.org/wiki/Hash-based_message_authentication_code

### p2p vpn failover

### vpn gateway failover
