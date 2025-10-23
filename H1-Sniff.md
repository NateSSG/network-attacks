# H1 Sniff | Nathaniel Ssendagire 23.10.2025

## Ympäristö

OS: Kali Linux

Browser: Firefox 128.3.1esr (64-bit)

Browser: Firefox 128.3.1esr (64-bit)

Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

Processor: AMD Ryzen 3 7320U - 4 cores used

Disk: 35 GB

Network: NAT

## Linux
Kali linuxen asennuksessa ei ilmennyt mitään ongelmia.

## Ei voi kalastaa

Tehtävässä piti näyttää että pystyn katkaisemaan ja palauttamaan virtuaalikoneen internet-yhteyden:

## Kuvat:

<img width="1717" height="995" alt="network adapter pic" src="https://github.com/user-attachments/assets/52da77a2-a0f4-4b2d-b5b8-f105a5d11f87" />





<img width="1722" height="992" alt="disconnect vm machine network" src="https://github.com/user-attachments/assets/c28fc1f9-edf2-4706-9803-737ddbe04f96" />


<img width="1721" height="997" alt="network not working" src="https://github.com/user-attachments/assets/55639867-51d0-4755-8f9d-d82b30b3b4a7" />


<img width="1712" height="1001" alt="network connection back" src="https://github.com/user-attachments/assets/e0164023-93ec-474a-9f42-c38b1a15ff80" />


<img width="1715" height="996" alt="network working pic" src="https://github.com/user-attachments/assets/d5201565-8bed-4465-80b9-36d6d21c0c73" />

## Wireshark
Wireshark on jo valmiiksi ladattuna Kali Linuxissa, joten minun ei tarvinnut tehdä sitä itse.

## Oikeesti TCP/IP

## 1) Linkkitason kerros (Ethernet) Phys/Link

Mitä laatikoida: Wiresharkissa Pakettilistan tai Packet Details paneelin kohta Ethernet II (Näkyy yleensä aivan ylimpänä)

Mitä siinä näkyy (esimerkistä)

- Lähde-MAC: bc:24:11:e9:27:b5 (PrroxmoxServe_e9:27:b5)
- Kohde-MAC: 00:0c:29:1b:77:cd (VMware_1b:77:cd)
- EtherType: 0x0800 (IPv4)

Lyhyt selitys: Linkkitaso kuljettaa kehyksen paikallisessa verkossa. MAC osoitteet kertovat lähettäjän ja vastaanottajan laitteet.

## 2) Internet/verkko-kerros (IPV4)

Mitä laatikoida: Packet Details paneelista Internet Protocol Version 4 lohko (yleensä heti Ethernetin alla).

Mitä siinä näkyy (esimerkistä):

- Lähde IP: 172.28.11.68
- Kohde IP: 172.28.172.64
- Protokolla: UDP (paketti kulkee IP:n sisällä)

Lyhyt selitys: IP-kerros vastaa reitityksestä ja osoitteista verkkojen välillä. Tässä kerroksessa kerrotaan kenelle paketti on tarkoitettu (IP-osoitteet).

## 3) Kuljetuskerros (UDP/TCP) Transport

Mitä laatikoida: Packet Details paneelin User Datagram Protocol tai (Transmission Control Protocol, jos oli TCP lohko).

Mitä siinä näkyy (esimerkistä):
- Lähdeportti 53 (DNS)
- Kohdeportti 53585 (ephemeral/asiakasportti)
- Pituus/Checksums (Wireshark näyttää)

Lyhyt selitys: Kuljetuskerros tarjoaa prosessien välisen liikenteen kanavat (portit). Tässä kyseessä on UDP-paketti, eli yhteydetön kuljetus (nopea, mutta ei varmista että paketti on saapunut kohteeseen).



