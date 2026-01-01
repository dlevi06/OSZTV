# CCNA1: Modul 1-3 Összefoglaló (Hálózati Alapok és IOS)

Ez az összefoglaló a hálózati architektúrák alapjait, a Cisco eszközök konfigurálását (IOS) és a rétegelt modelleket (OSI, TCP/IP) fedi le. Az OSZTV-n ez a "beugró" szint, de a számolások és a parancsok ismerete már itt is kritikus.

---

## 1. Modul: A hálózat jelene (Alapfogalmak)

### Hálózati típusok és szerepkörök
* **Végberendezések (Hosts):** Az üzenetek forrásai vagy céljai.
    * *Példák:* PC, laptop, szerver, okostelefon, IP telefon.
    * *Szerepkörök:* Kliens (ügyfél), Szerver (kiszolgáló).
* **Közvetítő eszközök (Intermediary Devices):** Összekötik a végberendezéseket és irányítják a forgalmat.
    * *Router (Forgalomirányító):* Hálózatokat köt össze (Layer 3), útvonalat választ.
    * *Switch (Kapcsoló):* Eszközöket köt össze egy hálózaton belül (Layer 2).
    * *Firewall (Tűzfal):* Szűri a forgalmat.
* **Közegek (Media):** Amin az adat utazik.
    * *Réz (Copper):* Elektromos impulzusok.
    * *Optika (Fiber):* Fényimpulzusok.
    * *Vezeték nélküli (Wireless):* Rádióhullámok.

### Hálózati topológiák
* **Fizikai topológia:** Hol helyezkednek el az eszközök és kábelek a valóságban (pl. melyik szobában, melyik rackben).
* **Logikai topológia:** Hogyan áramlik az adat, milyen az IP-címzés és a portkiosztás.

### Hálózati kategóriák (Méret szerint)
* **LAN (Local Area Network):** Kis földrajzi terület (otthon, iroda, épület). Nagy sávszélesség, saját felügyelet.
* **WAN (Wide Area Network):** Nagy földrajzi távolság (városok, országok között). Szolgáltató (ISP) biztosítja, lassabb, mint a LAN.
* **Internet:** A nyilvános hálózatok globális összessége.
* **Intranet:** Egy szervezet belső, zárt hálózata (csak dolgozóknak).
* **Extranet:** Az Intranet egy része, amit biztonságosan megnyitnak külső partnereknek (pl. beszállítóknak).

### Megbízható hálózatok 4 alappillére (OSZTV KRITIKUS!)
1.  **Hibatűrés (Fault Tolerance):** Redundancia (több útvonal). Ha egy eszköz/kábel kiesik, a forgalom átterelődik egy másik útra, és a szolgáltatás nem áll le.
2.  **Skálázhatóság (Scalability):** A hálózat bővíthető új felhasználókkal anélkül, hogy a meglévők teljesítménye romlana.
3.  **QoS (Quality of Service - Szolgáltatásminőség):** A forgalom priorizálása. Az időérzékeny adatok (hang, videó) elsőbbséget kapnak a kevésbé fontosakkal szemben (pl. e-mail, web).
4.  **Biztonság (Security):**
    * *Bizalmasság (Confidentiality):* Csak a címzett olvashatja.
    * *Integritás (Integrity):* Az adat nem módosult útközben.
    * *Rendelkezésre állás (Availability):* A szolgáltatás mindig elérhető.

---

## 2. Modul: Cisco IOS Alapbeállítások

### Hozzáférés és Üzemmódok
* **CLI (Command Line Interface):** Parancssoros felület.
* **Üzemmódok (Modes):**
    1.  **User EXEC Mode (`Switch>`):** Korlátozott, csak megtekintés.
    2.  **Privileged EXEC Mode (`Switch#`):** Teljes hozzáférés ("root"). Belépés: `enable`.
    3.  **Global Configuration Mode (`Switch(config)#`):** A teljes eszköz beállítása. Belépés: `configure terminal`.
    4.  **Line/Interface Mode (`Switch(config-if)#`):** Egy adott port vagy vonal beállítása.

### Alapvető parancsok (Ezeket be kell tudni gépelni!)
| Funkció | Parancs | Mód |
| :--- | :--- | :--- |
| **Név megadása** | `hostname S1` | Global Config |
| **Privilegizált jelszó (titkosított)** | `enable secret cisco` | Global Config |
| **Konzol jelszó** | `line console 0` -> `password cisco` -> `login` | Global Config -> Line |
| **VTY (Telnet/SSH) jelszó** | `line vty 0 4` -> `password cisco` -> `login` | Global Config -> Line |
| **Jelszavak titkosítása** | `service password-encryption` | Global Config |
| **Üzenet (Banner)** | `banner motd #No Access!#` | Global Config |
| **Mentés** | `copy running-config startup-config` | Privileged EXEC |

### IP címzés beállítása (SVI - Switch Virtual Interface)
Switchek menedzseléséhez (távoli eléréshez) IP-címet kell adni a virtuális VLAN interfésznek:
Switch(config)# interface vlan 1 Switch(config-if)# ip address 192.168.1.10 255.255.255.0 Switch(config-if)# no shutdown Switch(config)# ip default-gateway 192.168.1.1 <-- Hogy más hálózatból is elérhető legyen

---

## 3. Modul: Protokollok és Modellek

### Protokollkészletek (Protocol Suites)
A protokollok szabályok, amik meghatározzák a kommunikációt (formátum, időzítés, kódolás).
* **TCP/IP:** A mai internet szabványa (nyílt szabvány).
* **Szabványügyi szervezetek:** IEEE (Wi-Fi, Ethernet), IETF (Internet protokollok), ISO.

### Referenciamodellek (OSZTV - Sorbarendezés!)

| Réteg | OSI Modell (7 réteg) | TCP/IP Modell (4 réteg) | PDU (Adategység) | Funkció/Példa |
| :--- | :--- | :--- | :--- | :--- |
| **7.** | **Alkalmazási (Application)** | **Alkalmazási** | **Adat (Data)** | HTTP, DNS, DHCP, FTP (Interfész a felhasználónak) |
| **6.** | **Megjelenítési (Presentation)** | " | Adat | Kódolás, tömörítés, titkosítás (JPEG, MP3) |
| **5.** | **Viszony (Session)** | " | Adat | Kapcsolatok menedzselése |
| **4.** | **Szállítási (Transport)** | **Szállítási** | **Szegmens** | Megbízhatóság, portszámok (TCP, UDP) |
| **3.** | **Hálózati (Network)** | **Internet** | **Csomag (Packet)** | Logikai címzés (IP), útvonalválasztás |
| **2.** | **Adatkapcsolati (Data Link)** | **Hálózatelérési** | **Keret (Frame)** | Fizikai címzés (MAC), hibajavítás |
| **1.** | **Fizikai (Physical)** | " | **Bit** | Jelek továbbítása (kábel, rádióhullám) |

### Adatbeágyazás (Encapsulation)
Ahogy az adat lefelé halad a rétegeken, minden réteg hozzáadja a saját fejlécét (Header):
1.  **Adat** (App layer)
2.  **Szegmens** (Transport layer: Portszámok hozzáadása)
3.  **Csomag** (Network layer: IP címek hozzáadása)
4.  **Keret** (Data Link layer: MAC címek hozzáadása)
5.  **Bitek** (Physical layer: átvitel a közegen)

---

## OSZTV Kiemelt Ellenőrző kérdések (a megadott anyagból)

**1. Milyen típusú hálózatot használ egy cég, ha biztonságos hozzáférést ad a beszállítóinak a belső rendszeréhez?**
* *Válasz: Extranet.*

**2. Melyik hálózati jellemző biztosítja, hogy a videóhívás (időérzékeny adat) elsőbbséget élvezzen az e-maillel szemben?**
* *Válasz: QoS (Quality of Service).*

**3. Melyik parancs titkosítja az összes egyszerű szöveges jelszót a konfigurációs fájlban?**
* *Válasz: `service password-encryption`.*

**4. Mi a PDU neve a Hálózati rétegben (Network Layer)?**
* *Válasz: Csomag (Packet).*

**5. Melyik interfészen keresztül menedzselhető távolról egy Layer 2 switch?**
* *Válasz: SVI (Switch Virtual Interface), általában a VLAN 1.*

**6. Melyik billentyűkombináció szakítja meg a ping parancsot Cisco IOS-ben?**
* *Válasz: Ctrl-Shift-6.*

**7. Melyik két OSI réteg felel meg a TCP/IP Hálózatelérési (Network Access) rétegének?**
* *Válasz: Adatkapcsolati (Data Link) és Fizikai (Physical).*

---

# CCNA1: Modul 4-7 Összefoglaló (Fizikai réteg és Ethernet)

Ez a szakasz a hálózatok "vasát" (kábelek), a számrendszereket és az Ethernet működését (MAC-címek, switchek) fedi le.

---

## 4. Modul: Fizikai Réteg (Physical Layer)

### Feladata
* Bitek (0 és 1) átalakítása jelekké (elektromos, fény, rádió) és továbbítása a közegen.
* **Teljesítménymutatók:**
    * **Sávszélesség (Bandwidth):** Elméleti maximum (pl. 100 Mbps).
    * **Átbocsátóképesség (Throughput):** Ténylegesen átvitt adatmennyiség.
    * **Goodput:** Hasznos adat (fejlécek nélkül) adott idő alatt.

### Átviteli közegek
1.  **Rézkábel (Copper):** Elektromos jelek.
    * *UTP (Unshielded Twisted Pair):* Leggyakoribb (Ethernet). 4 érpár, csavarva az áthallás (Crosstalk) csökkentésére.
    * *STP:* Árnyékolt, ipari környezetbe (zavarvédelem).
    * *Koax:* KábelTV, antenna.
2.  **Optikai szál (Fiber):** Fényjelek.
    * Nagy távolság, nagy sávszélesség, immunis az elektromos zavarokra (EMI/RFI).
    * *Single-mode:* Lézer, nagy távolság. *Multi-mode:* LED, rövidebb távolság.
3.  **Vezeték nélküli (Wireless):** Rádióhullámok (Wi-Fi).

### Kábeltípusok (UTP)
* **Straight-through (Egyenes):** Különböző eszközök közé (pl. PC <-> Switch, Switch <-> Router).
* **Crossover (Kereszt):** Azonos eszközök közé (pl. PC <-> PC, Switch <-> Switch, Router <-> PC). *Megjegyzés: Modern eszközök az **Auto-MDIX** miatt felismerik maguktól.*
* **Rollover (Konzol):** PC és Router/Switch konzolportja közé (menedzseléshez).

---

## 5. Modul: Számrendszerek

### Bináris (Kettes)
* Csak 0 és 1.
* **Helyiértékek (8 biten):** 128, 64, 32, 16, 8, 4, 2, 1.
* *Példa:* `11000000` = 128 + 64 = **192**.

### Hexadecimális (Tizenhatos)
* 0-9 és A-F (A=10, B=11, ... F=15).
* Egy hex karakter = 4 bit.
* Használat: MAC-címek, IPv6 címek.
* *Példa:* `C0` (hex) -> `1100 0000` (bin) -> `192` (dec).

---

## 6. Modul: Adatkapcsolati Réteg (Data Link Layer)

### Feladata
* A hálózati csomagok **keretté (Frame)** alakítása.
* Fizikai címzés (MAC).
* Hibaellenőrzés (FCS/CRC).

### Alrétegek
1.  **LLC (Logical Link Control):** Szoftveres interfész a felső rétegek (IPv4/IPv6) felé.
2.  **MAC (Media Access Control):** Hardveres vezérlés, hozzáférés a közeghez.

---

## 7. Modul: Ethernet Kapcsolás

### Ethernet Keret (Frame) részei
1.  **Preamble:** Szinkronizáció.
2.  **Destination MAC:** Cél fizikai címe (hová megy?).
3.  **Source MAC:** Forrás fizikai címe (honnan jön?).
4.  **Type/Length:** Milyen protokoll van benne (pl. IPv4).
5.  **Data:** A hasznos adat (csomag).
6.  **FCS (Frame Check Sequence):** Hibaellenőrzés (CRC algoritmus). Ha hibás, a keretet eldobják.

### MAC Cím
* 48 bites (6 bájtos) egyedi azonosító.
* Formátum: `MM:MM:MM:UU:UU:UU` (első fele gyártó/OUI, második fele egyedi).
* **Broadcast MAC:** `FF:FF:FF:FF:FF:FF` (mindenkinek).

### Switch Működése (MAC tábla)
1.  **Learning (Tanulás):** A beérkező keret **Forrás MAC** címét eltárolja a porthoz rendelve.
2.  **Forwarding (Továbbítás):** Ha ismeri a **Cél MAC** címet, csak a megfelelő portra küldi.
3.  **Flooding (Elárasztás):** Ha NEM ismeri a célt (vagy Broadcast), kiküldi az összes porton (kivéve a bejövőn).

### Kapcsolási módok (Switching Modes)
* **Store-and-Forward:** A teljes keretet beolvassa és ellenőrzi a hibákat (FCS) továbbítás előtt. (Lassabb, de megbízhatóbb).
* **Cut-Through:**
    * *Fast-Forward:* Azonnal továbbít, amint megvan a Cél MAC. (Leggyorsabb, de hibát is továbbíthat).
    * *Fragment-Free:* Az első 64 bájtot (ütközési ablak) beolvassa, aztán továbbít.



## OSZTV Kiemelt Ellenőrző kérdések (a megadott anyagból)

**1. Mi a célja a fizikai rétegnek?**
* *Válasz: Bitek továbbítása a helyi közegen keresztül.*

**2. Melyik kábeltípusnál csökkentik az áthallást (crosstalk) a vezetékpárok összesodrásával?**
* *Válasz: UTP (Unshielded Twisted Pair).*

**3. Melyik kapcsolási mód (switching method) a leggyorsabb (legalacsonyabb késleltetés)?**
* *Válasz: Fast-forward (Cut-through).*

**4. Mi történik, ha a switch olyan keretet kap, aminek a Cél MAC címe nincs a táblájában?**
* *Válasz: Kiküldi az összes porton, kivéve azon, amelyiken beérkezett (Flooding).*

**5. Melyik MAC-cím a Broadcast cím?**
* *Válasz: FF:FF:FF:FF:FF:FF.*

**6. Milyen kábelt használunk egy PC és egy Switch összekötésére?**
* *Válasz: Straight-through (Egyenes).*

**7. Mi a feladata az FCS mezőnek az Ethernet keret végén?**
* *Válasz: Hibaellenőrzés (az átvitel során sérült-e az adat).*

---

# CCNA1: Modul 8-10 Összefoglaló (Hálózati Réteg és Routing)

Ez a szakasz az IP-címzést, a csomagok útvonalválasztását (routing), az ARP protokollt és a routerek alapvető konfigurálását tárgyalja. Az OSZTV-n ez a gyakorlati feladatok alapja.

---

## 8. Modul: Hálózati Réteg (Network Layer - OSI Layer 3)

### A protokoll jellemzői (IP)
* **Connectionless (Kapcsolatmentes):** Nem épít fel előzetes kapcsolatot a fogadóval (mint a levélfeladás).
* **Best Effort (Legjobb szándékú):** Nem garantálja a kézbesítést (ha hiba van, eldobja a csomagot; a TCP dolga az újraküldés).
* **Media Independent (Közegfüggetlen):** Nem számít, hogy rézkábelen, optikán vagy Wi-Fi-n megy az adat.

### IPv4 Csomag (Packet) Fejléc
* **TTL (Time to Live):** Élettartam. Minden router csökkenti az értékét 1-gyel. Ha eléri a 0-t, a csomagot eldobják (hurokvédelem).
* **Protocol:** Jelzi, mi van a csomag belsejében (pl. 6=TCP, 17=UDP, 1=ICMP).
* **Source / Destination IP:** Forrás és Cél IP-cím. **Fontos:** Ezek NEM változnak az út során (kivéve NAT esetén)!

### IPv6 Előnyei
* **Nincs szükség NAT-ra:** Elegendő cím áll rendelkezésre.
* **Egyszerűbb fejléc:** Nincs checksum mező, gyorsabb a feldolgozás a routereknek.
* **Hop Limit:** Ugyanaz a funkciója, mint az IPv4 TTL-nek.
* **Flow Label:** QoS támogatás (azonos folyamhoz tartozó csomagok jelölése).

### Routing Tábla (Routing Table)
A router (vagy PC) itt tárolja az útvonalakat. Parancs: `show ip route`.
* **C (Connected):** Közvetlenül csatlakozó hálózat.
* **L (Local):** A router saját interfészének IP-címe (/32).
* **S (Static):** Kézzel beírt útvonal.
* **O (OSPF), D (EIGRP):** Dinamikus routing protokollal tanult útvonal.
* **Gateway of Last Resort (Default Route):** Ha nincs találat a táblában, ide küldi a csomagot.

---

## 9. Modul: Címfeloldás (Address Resolution)

A kommunikációhoz két cím kell:
1.  **IP-cím (Logikai):** A végcél azonosítása (End-to-End).
2.  **MAC-cím (Fizikai):** A következő eszköz (Hop-to-Hop) azonosítása a helyi hálózaton.

### ARP (Address Resolution Protocol) - IPv4
* **Feladat:** Ismert IP-címhez a hozzá tartozó MAC-cím megtalálása.
* **Működés:**
    1.  **ARP Request:** Broadcast üzenet (`FF:FF:FF:FF:FF:FF`). *"Kinek van meg a 192.168.1.5 IP? Küldd el a MAC-címed!"* Mindenki megkapja.
    2.  **ARP Reply:** Unicast üzenet. Csak a keresett eszköz válaszol a saját MAC-címével.
    3.  **ARP Cache:** A válasz bekerül a memóriába, hogy ne kelljen újra kérdezni.
* **Probléma:** ARP Spoofing (mérgezés) – a támadó a saját MAC-címét hazudja a router helyett.

### Neighbor Discovery (ND) - IPv6
Az IPv6 **NEM használ ARP-t** (és Broadcastot sem). Helyette ICMPv6 üzeneteket használ:
* **Neighbor Solicitation (NS):** Keresés (mint az ARP Request).
* **Neighbor Advertisement (NA):** Válasz (mint az ARP Reply).
* **Router Solicitation (RS) / Advertisement (RA):** Routerek és előtagok automatikus felderítése (SLAAC).

---

## 10. Modul: Router Alapkonfiguráció

### A router indítása (Boot process)
1.  **POST:** Hardver teszt.
2.  **Bootstrap:** Betöltő program.
3.  **IOS betöltése:** Flash memóriából a RAM-ba.
4.  **Konfiguráció betöltése:** `startup-config` (NVRAM) -> `running-config` (RAM). Ha nincs startup-config, elindul a *Setup Mode*.

### Alapvető parancsok
| Funkció | Parancs | Mód |
| :--- | :--- | :--- |
| **Név** | `hostname R1` | Global Config |
| **Titkosított jelszó** | `enable secret cisco` | Global Config |
| **Jelszavak titkosítása** | `service password-encryption` | Global Config |
| **Mentés** | `copy running-config startup-config` | Privileged EXEC |

### Interfész konfigurálása
A router portjai alapértelmezésben ki vannak kapcsolva (`shutdown`).
```bash
Router(config)# interface gigabitethernet 0/0/1
Router(config-if)# description Link to LAN
Router(config-if)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# ipv6 address 2001:db8:acad:1::1/64
Router(config-if)# no shutdown  <-- Bekapcsolás!
```

Ellenőrzés (Show parancsok)
show ip interface brief: Gyors áttekintés (IP-k, Status: UP/UP).

show ip route: Routing tábla.

show interfaces: Részletes (MAC cím, hibák).

Alapértelmezett Átjáró (Default Gateway)
PC-n: A helyi router belső interfészének IP-címe. Ha ez rossz (vagy nincs), a PC nem éri el a távoli hálózatokat (Internetet), csak a helyi gépeket.

Switchen: ip default-gateway [IP] parancs kell, hogy a switch menedzsment felülete távolról elérhető legyen.

OSZTV Kiemelt Ellenőrző kérdések (a megadott tesztkérdések alapján)
1. Melyik információ alapján továbbítja a router a csomagot?

Válasz: A cél IP-cím alapján (Destination IP address).

2. Mi történik a TTL mezővel, ahogy a csomag áthalad egy routeren?

Válasz: Csökken egyel. Ha eléri a nullát, a csomagot eldobják.

3. Melyik IPv4 mező jelzi a felsőbb réteg protokollját (pl. TCP vagy UDP)?

Válasz: Protocol mező.

4. Mi a funkciója az ARP-nek?

Válasz: Az ismert IP-címhez megtalálni a MAC-címet a helyi hálózaton.

5. Milyen típusú üzenet az ARP Request?

Válasz: Broadcast (Szórás).

6. IPv6-ban mi helyettesíti az ARP-t?

Válasz: ICMPv6 Neighbor Discovery (ND).

7. Melyik parancs menti a konfigurációt, hogy újraindítás után is megmaradjon?

Válasz: copy running-config startup-config.

8. Mi a teendő, ha a routeren a show ip interface brief parancsra az interfész állapota "Administratively Down"?

Válasz: Ki kell adni a no shutdown parancsot az interfész konfigurációs módban.

9. Ha egy PC eléri a helyi fájlszervert, de az internetet nem, mi a legvalószínűbb hiba?

Válasz: Rossz vagy hiányzó Alapértelmezett Átjáró (Default Gateway) beállítás a PC-n.

---

# CCNA1: Modul 11-13 Összefoglaló (IP Címzés és ICMP)

Ez a szakasz az IP-címzést, az alhálózatosítást (subnetting) és az ICMP protokollt tárgyalja. Ez a legfontosabb számolós rész az OSZTV-n.

---

## 11. Modul: IPv4 Címzés és Alhálózatosítás

### IPv4 Cím Felépítése
* 32 bites cím, 4 oktettben (pl. 192.168.10.5).
* **Hálózati rész (Network Portion):** Azt mutatja meg, melyik hálózathoz tartozik az eszköz.
* **Állomás rész (Host Portion):** Az egyedi eszközt azonosítja a hálózaton belül.
* **Alhálózati maszk (Subnet Mask):** Megmondja, hol a határ a kettő között (ahol 1-esek vannak, az a hálózat).
    * Pl. `/24` = 255.255.255.0 (első 24 bit a hálózat).

### Címtípusok
* **Network Address (Hálózati cím):** Az első cím (Host bitek = 0). Nem osztható ki eszköznek.
* **Broadcast Address (Szórási cím):** Az utolsó cím (Host bitek = 1). Mindenkinek szól.
* **Host Address:** A kettő közötti címek, amik kioszthatók.

### Alhálózatosítás (Subnetting) - A Számolás Alapja
Hogyan daraboljunk fel egy hálózatot kisebbekre?
* **Képlet (Alhálózatok száma):** $2^n$ (ahol n = kölcsönvett bitek száma).
* **Képlet (Hostok száma):** $2^h - 2$ (ahol h = megmaradt host bitek száma; a -2 a Network és Broadcast cím miatt kell).

**Példa:** `/24`-ből csináljunk `/26`-os alhálózatokat.
* Eredeti maszk: 24 bit. Új maszk: 26 bit.
* Kölcsönvett bitek: $26 - 24 = 2$.
* Alhálózatok száma: $2^2 = 4$.
* Host bitek maradtak: $32 - 26 = 6$.
* Hostok száma per alhálózat: $2^6 - 2 = 64 - 2 = 62$.

### Privát Címtartományok (RFC 1918)
Ezeket az Interneten NEM továbbítják (csak belső hálózaton használhatók, NAT kell hozzájuk):
* **10.0.0.0/8**
* **172.16.0.0/12**
* **192.168.0.0/16**

---

## 12. Modul: IPv6 Címzés

### IPv6 Jellemzői
* **128 bites cím** (hexadecimális, 8 hextet).
* Nincs Broadcast, helyette **Multicast** van.
* Címtípusok:
    * **GUA (Global Unicast Address):** Publikus, Interneten routolható (2000::/3).
    * **LLA (Link-Local Address):** Csak a helyi hálózaton érvényes, nem routolható. Minden interfésznek kötelezően van! (`FE80::/10`).
    * **Multicast:** Csoportcím (`FF00::/8`). Pl. `FF02::1` (Minden node), `FF02::2` (Minden router).

### IPv6 Tömörítési Szabályok (OSZTV kedvenc!)
1.  **Vezető nullák elhagyhatók:** `00AB` -> `AB`.
2.  **Dupla kettőspont (::):** Egymást követő csupa 0 hextetek helyettesíthetők vele, de **CSAK EGYSZER** egy címben!
    * *Példa:* `2001:0db8:0000:0000:0000:0000:0000:0001` -> `2001:db8::1`.

### Címkiosztás
* **SLAAC (Stateless Address Auto-Configuration):** A router (RA üzenetben) megadja a prefixet, a host pedig generál magának egy Interface ID-t (pl. EUI-64 módszerrel a MAC-címből).

---

## 13. Modul: ICMP (Internet Control Message Protocol)

### Feladata
Hibaüzenetek és diagnosztika az IP hálózatban. Nem adatátvitelre való!

### Eszközök
* **Ping:** Elérhetőség tesztelése (Echo Request / Echo Reply).
* **Traceroute (tracert):** Útvonal felderítése. A TTL (IPv4) vagy Hop Limit (IPv6) mezőt használja. Amikor a TTL lejár (0 lesz), a router visszaküld egy "Time Exceeded" üzenetet.

---

## OSZTV Kiemelt Ellenőrző kérdések (a megadott anyagból)

**1. Hány használható host cím van egy /26-os alhálózaton?**
* *Számolás:* 32 - 26 = 6 host bit. $2^6 - 2 = 64 - 2 = 62$.
* *Válasz: 62.*

**2. Melyik a helyes tömörített alakja a `2001:0db8:0000:0000:0000:a0b0:0008:0001` címnek?**
* *Válasz: `2001:db8::a0b0:8:1`.*

**3. Melyik IPv6 címtípus kezdődik `FE80`-nal?**
* *Válasz: Link-Local Address (LLA).*

**4. Mire való a `ping 127.0.0.1` parancs?**
* *Válasz: Loopback teszt. Ellenőrzi, hogy a TCP/IP stack (driver) működik-e a saját gépen.*

**5. Melyik parancs mutatja meg az útvonalat a célig és az egyes ugrások (hopok) idejét?**
* *Válasz: `tracert` (Windows) vagy `traceroute` (Cisco).*

**6. Melyik IPv4 cím nem routolható az Interneten (Privát cím)?**
* *Válasz: Pl. `192.168.x.x`, `10.x.x.x`, `172.16.x.x`.*

**7. Mi a Broadcast cím a `192.168.1.0/24` hálózatban?**
* *Válasz: `192.168.1.255`.*

---

# CCNA1: Modul 14-15 Összefoglaló (Szállítási és Alkalmazási Réteg)

Ez a szakasz a hálózati kommunikáció "agyát" (TCP/UDP) és a felhasználói felülethez legközelebbi réteget (HTTP, DNS, DHCP) tárgyalja.

---

## 14. Modul: Szállítási Réteg (Transport Layer - OSI Layer 4)

### Feladata
* **Szegmentálás:** A nagy adatokat kisebb darabokra (szegmensekre) bontja.
* **Multiplexelés:** Több alkalmazás (pl. böngésző + Spotify) adatait keveri egy adatfolyamba.
* **Azonosítás:** Portszámokkal különbözteti meg az alkalmazásokat.

### Két fő protokoll: TCP vs UDP (OSZTV kedvenc!)

| Jellemző | TCP (Transmission Control Protocol) | UDP (User Datagram Protocol) |
| :--- | :--- | :--- |
| **Megbízhatóság** | **Igen** (Nyugtázás, újraküldés) | **Nem** (Best Effort) |
| **Kapcsolat** | **Kapcsolatalapú** (Connection-oriented) | **Kapcsolatmentes** (Connectionless) |
| **Sorrend** | Helyreállítja (Sorszámozás) | Nem (Ahogy esik, úgy puffan) |
| **Sebesség** | Lassabb (Több adminisztráció) | Gyorsabb (Kisebb fejléc) |
| **Felhasználás** | Web, E-mail, Fájlátvitel | Videó, Hang (VoIP), DNS, DHCP |

### TCP Működése
* **3-way Handshake (Háromutas kézfogás):** Kapcsolatfelépítés.
    1.  **SYN:** "Beszélhetünk?"
    2.  **SYN-ACK:** "Igen, beszélhetünk."
    3.  **ACK:** "Rendben, kezdjük."
* **Flow Control (Ablakméret - Window Size):** A fogadó jelzi, mennyi adatot tud egyszerre befogadni. Ha a hálózat lassú, csökkenti az ablakméretet.

### Portszámok (Port Numbers)
Azonosítják a futó szolgáltatást.
* **Well-known Ports (0-1023):** Szabványos szolgáltatások (pl. HTTP=80).
* **Registered Ports (1024-49151):** Regisztrált alkalmazások.
* **Dynamic/Private Ports (49152-65535):** Kliensek használják ideiglenesen (forrás port).

---

## 15. Modul: Alkalmazási Réteg (Application Layer - OSI Layer 7)

Ez a réteg biztosítja a felületet a felhasználói programok és a hálózat között.

### Web és E-mail Protokollok
* **HTTP (80):** Weboldalak letöltése (szöveges, nem biztonságos).
* **HTTPS (443):** Titkosított web (SSL/TLS).
* **SMTP (25):** E-mail **küldése** (szerverek között vagy klienstől szervernek).
* **POP3 (110):** E-mail **letöltése** (letölti és törli a szerverről).
* **IMAP (143):** E-mail **szinkronizálása** (a levél a szerveren marad).

### IP Címzési Szolgáltatások
* **DNS (Domain Name System - 53):** Névfeloldás. A könnyen megjegyezhető neveket (www.google.com) IP-címekké alakítja.
    * *Record típusok:* **A** (IPv4), **AAAA** (IPv6), **MX** (Mail Exchange).
* **DHCP (Dynamic Host Configuration Protocol - 67/68):** Automatikus IP-cím kiosztás.
    * *Folyamat (DORA):* **D**iscover -> **O**ffer -> **R**equest -> **A**cknowledge.

### Fájlátvitel
* **FTP (File Transfer Protocol - 20/21):** Fájlok másolása. Két csatornát használ (egy vezérlésre, egy adatra).
* **SMB (Server Message Block):** Windows fájlmegosztás és nyomtatás.

---

## OSZTV Kiemelt Ellenőrző kérdések (a megadott anyagból)

**1. Melyik protokoll használ "3-utas kézfogást" (3-way handshake) a kapcsolat felépítéséhez?**
* *Válasz: TCP.*

**2. Melyik mező segít a TCP-nek a csomagok helyes sorrendjének visszaállításában?**
* *Válasz: Sequence Number (Sorszám).*

**3. Mi a feladata a DNS szervernek?**
* *Válasz: A domain neveket (URL) IP-címekké fordítja.*

**4. Melyik protokoll alkalmas videó streamingre vagy VoIP hívásra, ahol a sebesség fontosabb a pontosságnál?**
* *Válasz: UDP.*

**5. Melyik portszámot használja a biztonságos webböngészés (HTTPS)?**
* *Válasz: 443.*

**6. Melyik e-mail protokoll tölti le a levelet a szerverről és törli azt onnan alapértelmezésben?**
* *Válasz: POP3.*

**7. Mi a Socket?**
* *Válasz: Az IP-cím és a Portszám kombinációja (pl. 192.168.1.10:80), ami egy adott folyamatot azonosít.*

**8. Mi a DHCP Discover üzenet célja?**
* *Válasz: A kliens keres egy DHCP szervert a hálózaton, hogy IP-címet kérjen.*

---

# CCNA1: Modul 16-17 Összefoglaló (Biztonság és Kis Hálózatok)

Ez a szakasz a hálózatbiztonság alapjait, a fenyegetéseket, a védekezést és a kisméretű hálózatok építését/hibaelhárítását tárgyalja.

---

## 16. Modul: Hálózatbiztonsági Alapok

### Biztonsági Fenyegetések (Threats)
* **Információlopás:** Adatok ellopása (pl. jelszavak, kártyaadatok).
* **Adatvesztés/Manipuláció:** Adatok törlése vagy megváltoztatása.
* **Személyazonosság-lopás (Identity Theft):** Más nevében elkövetett cselekmények.
* **Szolgáltatásmegtagadás (DoS - Denial of Service):** A hálózat vagy szerver megbénítása túlterheléssel.

### Támadási Típusok
* **Reconnaissance (Felderítés):** Információgyűjtés a hálózatról (pl. `nslookup`, `ping sweep`, port scan).
* **Access (Hozzáférés):** Jogosulatlan behatolás (pl. jelszófeltörés, Man-in-the-Middle).
* **DoS / DDoS:** Szolgáltatás megbénítása.

### Kártevők (Malware) - Ismétlés
* **Vírus:** Emberi beavatkozással terjed (fájlhoz csatolva).
* **Féreg (Worm):** Önmagát sokszorosítja a hálózaton (ember nélkül).
* **Trójai:** Hasznosnak tűnő program, ami kártékony kódot rejt.

### Védekezési Módszerek
* **Defense-in-Depth (Mélységi védelem):** Többrétegű védelem (Tűzfal + IPS + Vírusirtó + Házirend).
* **AAA:**
    1.  **Authentication (Hitelesítés):** Ki vagy? (Jelszó).
    2.  **Authorization (Jogosultság):** Mit tehetsz? (Hozzáférési jogok).
    3.  **Accounting (Naplózás):** Mit tettél? (Logfájlok).
* **Tűzfal (Firewall):**
    * *Packet Filtering:* IP/Port alapú szűrés.
    * *Stateful Inspection:* Figyeli a kapcsolat állapotát (csak a belső kérésre érkező választ engedi be).

### Eszközök védelme (Device Hardening)
* Alapértelmezett jelszavak megváltoztatása.
* **SSH** használata Telnet helyett (titkosított).
* Jelszavak titkosítása (`service password-encryption`).
* **Login block:** Brute-force támadás elleni védelem (pl. 3 hibás kísérlet után kitiltás). Parancs: `login block-for [mp] attempts [szám] within [mp]`.

---

## 17. Modul: Kisméretű Hálózat Építése és Hibaelhárítás

### Tervezés és Protokollok
* **Redundancia:** Tartalék eszközök/linkek a hibatűrés érdekében.
* **QoS (Quality of Service):** A valós idejű forgalom (VoIP, Videó) priorizálása.
* **Alkalmazások:** HTTP, FTP, SMTP, DNS, DHCP.

### Kapcsolatok Ellenőrzése és Parancsok (OSZTV KRITIKUS!)
* **Ping:** Elérhetőség tesztelése (ICMP).
    * `ping 127.0.0.1`: Loopback (saját TCP/IP stack) teszt.
* **Traceroute (`tracert` Windowsban):** Útvonal követése, hiba helyének megtalálása.
* **Cisco IOS parancsok:**
    * `show ip interface brief`: Interfészek állapota (L1/L2).
    * `show ip route`: Routing tábla.
    * `show cdp neighbors (detail)`: Szomszédos Cisco eszközök felderítése (IP cím, típus, interfész).
    * `show version`: IOS verzió, uptime, regiszterek.
    * `show running-config`: Aktuális konfiguráció.

### Hibaelhárítási Módszerek (Troubleshooting)
1.  **Probléma azonosítása.**
2.  **Elmélet felállítása (Mi okozhatja?).**
3.  **Tesztelés.**
4.  **Megoldás.**
5.  **Ellenőrzés.**
6.  **Dokumentálás.**

### Hibatípusok
* **Duplex Mismatch:** Egyik oldal Full, másik Half duplex -> Lassú hálózat, ütközések (Collision).
* **IP Címzés:** Hibás IP, Maszk vagy Gateway -> Nincs elérés.
* **DNS Hiba:** Névvel nem megy (`ping www.cisco.com`), de IP-vel igen (`ping 8.8.8.8`).

---

## OSZTV Kiemelt Ellenőrző kérdések (a megadott anyagból)

**1. Melyik protokoll biztosít titkosított (biztonságos) távoli hozzáférést a router parancssorához?**
* *Válasz: SSH (Secure Shell).*

**2. Mi a különbség a Vírus és a Féreg között?**
* *Válasz: A vírusnak emberi beavatkozás (fájlnyitás) kell, a féreg önállóan terjed a hálózaton.*

**3. Melyik parancs mutatja meg a szomszédos Cisco eszközök részletes adatait (pl. IP cím)?**
* *Válasz: `show cdp neighbors detail`.*

**4. Mi a teendő, ha a `ping 127.0.0.1` sikeres, de a `ping [gateway]` nem?**
* *Válasz: A TCP/IP stack jó, de hálózati probléma van (kábel, IP beállítás, router hiba).*

**5. Milyen típusú támadás az, amikor a támadó a hálózatot pásztázza nyitott portok után?**
* *Válasz: Reconnaissance (Felderítés).*

**6. Melyik parancs védi a routert a jelszó-találgatásos (brute-force) támadások ellen?**
* *Válasz: `login block-for ...` (Login block).*

**7. Melyik parancs teszi lehetővé, hogy a távoli (SSH/Telnet) munkamenetben is lássuk a log üzeneteket?**
* *Válasz: `terminal monitor`.*
