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

Lyhyt selitys: Kuljetuskerros vastaa prosessien välisestä tiedonsiirrosta käyttämällä porttinumeroita. Tässä tapauksessa kyseessä on UDP-paketti, joka on yhteydetön kuljetusprotokolla. Se on nopea, mutta ei takaa että paketti saapuu perille. UDP sopii erityisen hyvin sovelluksiin, joissa reaaliaikaisuus on tärkeämpää kuin virheenkorjaus, kuten Twitchin, Zoomin, Teamsin tai verkkopelien tiedonsiirrossa.

## 4) Sovelluskerros (DNS) Application

Mitä laatikoida: Packet Details paneelin Domain Name System (response) lohko ja Packet Bytes näytöstä DNS payload (kysely/vastausosat).

Mitä siinä näkyy (esimerkistä):
- DNS vastaa kyselyyn terokarvinen.goatcounter.com
- A: 65.21.71.180
- AAAA: 2a01:4f9:3081:5413::2

Lyhyt selitys: Sovelluskerros sisältää itse protokollan tietosisällön. Tässä DNS kertoo domainin IP-osoitteet, joita selain tai muu sovellus käyttää.

## Kuva


<img width="1677" height="800" alt="503617626-ed34b3ab-7922-46cc-a494-502495f4345f" src="https://github.com/user-attachments/assets/65a22e47-8c1a-47d4-ae8b-b02ffb5e5749" />

## Mitäs tuli surffattua?

Kaappaus näyttää tavallista verkkoselaamista. Käyttäjä (todennäköisesti Tero Karvinen) vierailee omalla verkkosivullaan terokarvinen.com ja tämän jälkeen tekee yhteyden ulkopuoliseen palveluun goatcounter.com, joka toimii todennäköisesti kävijäseurannan tai analytiikan työkaluna.

Aluksi näkyy DNS-kyselyitä, joissa selvitetään kyseisten verkkotunnusten IP-osoitteita. Tämän jälkeen liikenne jatkuu HTTPS-yhteytenä (TLS:n ja TCP:n kautta), mikä viittaa suojattuun verkkoselailuun. Kaappauksessa esiintyy myös ARP-paketteja, joissa paikallinen laite kysyy verkon toisen laitteen (192.168.122.7) MAC-osoitetta – tämä on normaalia lähiverkon toimintaa.

Kaappauksen loppupuolella näkyy useita ACK- ja FIN-paketteja, jotka kertovat yhteyksien päättämisestä.

Yhteenvetona voidaan todeta, että kyseessä on lyhyt verkkoselailusessio, jossa käyttäjä avaa oman sivustonsa ja hyödyntää sen yhteydessä ulkoista palvelua. Liikenne on täysin normaalia ja tyypillistä selaimen toimintaa.


<img width="1685" height="298" alt="google browser showing in wireshark" src="https://github.com/user-attachments/assets/970d23c8-1c20-4a77-a8db-70541911b8f8" />

<img width="1402" height="67" alt="terokarvinen web page showing in wireshark" src="https://github.com/user-attachments/assets/d95f55f4-b74d-44a4-935c-5f5d7cf99231" />

<img width="1666" height="142" alt="goatcounter with teros name in it" src="https://github.com/user-attachments/assets/c04f7144-1e88-4878-af8a-16b86bdc2f93" />

<img width="1153" height="41" alt="arp request" src="https://github.com/user-attachments/assets/615cfd3b-da99-4be7-8563-3ea7900ad25b" />





<img width="1697" height="672" alt="amount of devices on the wireshark capture" src="https://github.com/user-attachments/assets/7406f619-72a2-4479-9ccf-c8d1f6770d32" />

<img width="1367" height="738" alt="protocols we could find " src="https://github.com/user-attachments/assets/ca059b0d-8d1d-4672-9e01-e1d68318067a" />

<img width="1140" height="900" alt="packet size and how long it took" src="https://github.com/user-attachments/assets/566b1b53-d0ed-417b-b274-7682cd6b4887" />

##  Mitä selainta käyttäjä käyttää?

Ensiksi suodatettiin TLS:stä vain Client Hello -paketit. Tämä on se handshake, jossa selain ja palvelin käytännössä tekevät “sopimuksen” kommunikoida keskenään. Sitten järjestin paketit aikajärjestykseen ja valitsin ensimmäisen handshake-paketin, koska se on alkuperäinen viesti ja paras fingerprintin muodostamiseen.

Sitten laajensin Wiresharkissa Transport Layer Security → Handshake Protocol → Client Hello ja katsoin kaikki Cipher Suites, Extensions ja TLS-versiot — kaiken sen, mitä selain kertoo tuetuista asetuksistaan. Nämä kentät muodostavat niin sanotun JA3-fingerprintin.

Seuraavaksi kopioin fingerprintin ja käytin ja3.com-sivustoa vertaamaan sitä tunnetuihin fingerprintteihin. Sivusto antoi todennäköisen selaimen, jota käytettiin.

<img width="1720" height="970" alt="handhsake protocol filtering tls" src="https://github.com/user-attachments/assets/96cbe72a-96e5-47e5-a7ff-8a73d9854220" />


<img width="857" height="368" alt="copied the hash of the handshake" src="https://github.com/user-attachments/assets/fd327512-2ee3-4009-ad1c-6ed05a7f1d66" />


<img width="1716" height="968" alt="ja3 helped in this case" src="https://github.com/user-attachments/assets/92e9733b-c243-401a-9632-ba2f614e3602" />

## Minkä merkkinen verkkokortti käyttäjällä on

Kun katsoin Ethernet-kerrosta Wiresharkissa, sieltä näkee yleensä käyttäjän verkkokortin (NIC). Tavallisesti Wireshark näyttää sekä MAC-osoitteen että kortin valmistajan/merkin. Tässä kaappauksessa se kuitenkin näytti vain MAC-osoitteen, ei merkkiä.

Jotta sain lisätietoa, kopioin MAC-osoitteen ja käytin MAC-vendor lookup -työkalua. Työkalu arveli, että kyseessä voisi olla Realtek-verkkokortti, mutta varmuutta ei ole. Syynä tähän voi olla se, että MAC-osoite on paikallisesti määritetty (LAA), eli käyttöjärjestelmä tai virtuaalikone on asettanut sen itse. Se voi myös olla satunnaistettu, mikä on nykyaikainen yksityisyyden suojaus, joka estää laitteen seurannan.

Eli käytännössä tiedämme, mikä MAC-osoite kuuluu kortille, mutta emme voi varmasti sanoa valmistajaa, koska kyseessä voi olla LAA, MAC-randomisointi tai virtuaalinen verkkokortti.

<img width="1653" height="567" alt="network card" src="https://github.com/user-attachments/assets/fb7c687c-42c7-4699-8f0f-88561f2110ce" />

<img width="1213" height="303" alt="mac address lookup" src="https://github.com/user-attachments/assets/9c2bb341-5072-4d59-9a94-d69bcbf2ee4e" />

<img width="1130" height="429" alt="potential realtek nic" src="https://github.com/user-attachments/assets/e16c8949-b073-493d-b51a-e197e4950e90" />

## Millä weppipalvelimella käyttäjä on surffaillut?

Ensiksi suodatettiin kaappaus Wiresharkissa käyttämällä dns.qry.name -filtteriä, jotta näkyville tulisivat vain DNS-kyselyt eli käytännössä kaikki verkkosivut, joille käyttäjä yritti mennä. Tämän avulla oli helppo nähdä, mitä web-palvelimia hän käytti. Lista sisälsi terokarvinen.com, goatcounter.com, gc.zgo.at ja google.com.

Kyselyjen järjestyksen perusteella näkee, että käyttäjä meni ensin Googleen, kopioi linkin ja laittoi sen GoatCounter.com-palveluun saadakseen tilastot. GoatCounter on verkkosivustojen kävijäseurantapalvelu, joka kerää tietoa siitä, kuinka monta ihmistä käy sivustolla, mistä he tulevat ja mitä sivuja he katsovat. gc.zgo.at toimii tässä GoatCounterin välityspalvelimena.

Eli kaappauksesta näkyy tarkasti käyttäjän tekemät verkkopyynnöt ja se, miten hän käytti omaa sivustoaan yhdessä GoatCounterin kanssa saadakseen tilastoja.

<img width="1688" height="457" alt="dns query name thign" src="https://github.com/user-attachments/assets/f409f50e-4281-45e9-8050-5d61c5d01269" />

##  Analyysi. Sieppaa pieni määrä omaa liikennettäsi

Avasin terminaalin ja pingasin Googlea (8.8.8.8) ja samalla käytin Wiresharkia seuraamaan liikennettä. Kaappauksesta näkyi selvästi kaikki ICMP Echo Request -paketit (pingit) ja Echo Reply -paketit (vastaukset) kulkemassa edestakaisin.

Wireshark näytti lähde- ja kohdeosoitteet, pakettien pituudet sekä sekvenssinumerot ja TTL-arvot, mikä auttaa näkemään, miten paketit kulkevat verkon läpi ja kuinka nopeasti ne saavat vastauksen. Tämä oli hyvä esimerkki yhteydettömästä liikenteestä, jossa näkyy pelkästään kysely ja vastaus ilman varsinaista dataa.

Lisäksi kaappauksesta näkyi myös muita protokollia, kuten ARP ja UDP, jotka ovat osa normaalia verkon toimintaa. ICMP-pakettien perusteella pystyin helposti seuraamaan omaa verkkoliikennettäni ja varmistamaan, että ping-toiminnot toimivat odotetusti.


<img width="651" height="378" alt="pinging from terminal" src="https://github.com/user-attachments/assets/02dbc07a-df87-4377-8b46-3144e1b285bf" />



<img width="1712" height="682" alt="pingin google dns" src="https://github.com/user-attachments/assets/f4af1beb-5066-4512-acbb-9c8e2727fbaf" />






