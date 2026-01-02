# CCNA 3: Modul 1-3 Összefoglaló (OSPF és Kiberbiztonság)

Ez a szakasz a nagyvállalati hálózatok legfontosabb dinamikus routing protokollját (OSPF) és a modern kiberbiztonsági fenyegetéseket tárgyalja.

---

## 1. Modul: Egyterületű OSPFv2 Alapfogalmak

### OSPF Jellemzői
* **Link-State Protokoll:** Ismeri a teljes hálózati topológiát (térképet készít).
* **Algoritmus:** Dijkstra SPF (Shortest Path First) algoritmus.
* **Metrika (Metric):** Költség (Cost), ami a sávszélességtől függ (gyorsabb link = kisebb költség = jobb út).
* **Adminisztratív Távolság (AD):** 110.

### OSPF Csomagtípusok
1.  **Hello:** Szomszédok (Neighbors) felfedezése és kapcsolat fenntartása (Keepalive).
2.  **DBD (Database Description):** Az adatbázis tartalomjegyzéke.
3.  **LSR (Link-State Request):** Hiányzó információ kérése.
4.  **LSU (Link-State Update):** A kért információ (LSA-k) küldése.
5.  **LSAck:** Nyugtázás.

### OSPF Működése
* **Állapotok (States):** Down -> Init -> Two-Way -> ExStart -> Exchange -> Loading -> **Full** (Kész).
* **DR/BDR (Designated Router):** Többes hozzáférésű (Multiaccess, pl. Ethernet) hálózaton választják, hogy csökkentsék a forgalmat. Mindenki csak a DR-nek küld infót, ő szórja szét.
    * *Választás:* 1. Legmagasabb Priority, 2. Legmagasabb Router ID.

---

## 2. Modul: Egyterületű OSPFv2 Konfigurálása

### Router ID (RID)
Egyedi azonosító (32 bites, mint egy IPv4 cím).
* **Sorrend:**
    1.  Kézzel beállított (`router-id x.x.x.x`).
    2.  Legmagasabb Loopback IP.
    3.  Legmagasabb fizikai interfész IP (ami UP).

### Alapkonfiguráció (Példa)
```bash
router ospf 10                     <-- Process ID (helyi jelentőségű)
 router-id 1.1.1.1                 <-- Router ID beállítása
 network 192.168.10.0 0.0.0.255 area 0  <-- Hálózat hirdetése (Wildcard maszk!)
 network 10.1.1.0 0.0.0.3 area 0
 passive-interface g0/0            <-- LAN felé ne küldjön Hello-t!
 auto-cost reference-bandwidth 1000 <-- Gigabit hálózatokhoz (opcionális)

```

### Ellenőrzés

* `show ip ospf neighbor`: Szomszédok állapota (Full/DR, Full/BDR, vagy 2-Way/DROther).
* `show ip route ospf`: Csak az OSPF által tanult utak (O betűvel jelölve).
* `show ip protocols`: OSPF beállítások (RID, hirdetett hálózatok).

### Költség (Cost) módosítása

Ha manuálisan akarjuk befolyásolni az útvonalat:

```bash
interface g0/0
 ip ospf cost 50

```

---

## 3. Modul: Kiberbiztonság és Fenyegetések

### Támadók Típusai

* **White Hat:** Etikus hacker (tesztel, véd).
* **Black Hat:** Kiberbűnöző (lop, rombol).
* **Grey Hat:** A kettő között (engedély nélkül tesztel, de nem feltétlenül árt).
* **Hacktivist:** Politikai/társadalmi célok.
* **State-Sponsored:** Állami támogatású (kiberhadviselés).

### Támadási Eszközök és Módszerek

* **Malware:**
* *Virus:* Fájlhoz tapad, emberi indítás kell.
* *Worm:* Önállóan terjed a hálózaton.
* *Trojan:* Álcázott kártevő.
* *Ransomware:* Zsarolóvírus (titkosít).


* **Reconnaissance (Felderítés):** Port scan, Ping sweep.
* **Access (Hozzáférés):** Jelszófeltörés, MITM (Man-in-the-Middle).
* **DoS/DDoS:** Szolgáltatásmegtagadás (túlterhelés).

---

## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik OSPF csomag szolgál a szomszédok felfedezésére és a kapcsolat fenntartására?**

* *Válasz: Hello csomag.*

**2. Mi alapján választják a DR-t (Designated Router) OSPF-ben?**

* *Válasz: 1. Legmagasabb OSPF Priority. 2. Legmagasabb Router ID.*

**3. Melyik parancs mutatja meg az OSPF szomszédokat és állapotukat?**

* *Válasz: `show ip ospf neighbor`.*

**4. Mi a Wildcard maszkja a /24-es alhálózatnak (255.255.255.0)?**

* *Válasz: 0.0.0.255 (a subnet maszk inverze).*

**5. Melyik OSPF állapot jelzi, hogy az adatbázisok teljesen szinkronban vannak?**

* *Válasz: Full state.*

**6. Mi a passzív interfész (passive-interface) feladata OSPF-ben?**

* *Válasz: Nem küld ki Hello üzeneteket azon a porton (biztonság és sávszélesség-kímélés a LAN felé).*

**7. Milyen típusú hacker az, aki engedély nélkül hatol be, de nem okoz kárt, hanem nyilvánosságra hozza a hibát?**

* *Válasz: Grey Hat (Szürke kalapos).*

---

# CCNA 3: Modul 4-5 Összefoglaló (ACL - Hozzáférés-vezérlő Listák)

Ez a szakasz a hálózati forgalom szűrésének alapjait, a **Standard** és **Extended** ACL-ek közötti különbséget, valamint azok konfigurálását tárgyalja. Az OSZTV-n ez a gyakorlati feladatok egyik legfontosabb biztonsági eleme.

---

## 4. Modul: ACL Alapfogalmak

### Mi az ACL (Access Control List)?
Szabályok (ACE - Access Control Entries) sorozata, amelyek engedélyezik (`permit`) vagy tiltják (`deny`) a forgalmat a router interfészén.
* **Irány:** Minden interfészhez rendelhető egy ACL bejövő (`in`) és egy kimenő (`out`) irányban (protokollonként).
* **Implicit Deny:** Minden ACL végén van egy láthatatlan *"minden mást tilts le"* szabály. Ha egy csomag egyik fenti szabályra sem illeszkedik, eldobásra kerül.

### ACL Típusok
1.  **Standard ACL:**
    * **Szűrés:** Csak a **Forrás IP-cím** alapján.
    * **Számozás:** 1-99 (és 1300-1999).
    * **Elhelyezés:** A lehető legközelebb a **CÉLHOZ** (mivel mindent blokkol a forrástól, nem akarjuk korán kiszűrni).
2.  **Extended (Kiterjesztett) ACL:**
    * **Szűrés:** Forrás IP, Cél IP, Protokoll (TCP/UDP/ICMP), Portszám (pl. 80, 443).
    * **Számozás:** 100-199 (és 2000-2699).
    * **Elhelyezés:** A lehető legközelebb a **FORRÁSHOZ** (hogy a felesleges forgalom ne terhelje a hálózatot).

### Wildcard Maszk (Helyettesítő Maszk)
Az ACL-ek nem alhálózati maszkot, hanem Wildcard maszkot használnak (a maszk inverze).
* **0:** A bitnek egyeznie kell.
* **1:** A bit bármi lehet (nem számít).
* **Számítás:** `255.255.255.255 - Subnet Mask = Wildcard Mask`.
    * Pl. `/24` (255.255.255.0) -> Wildcard: `0.0.0.255`.
* **Kulcsszavak:**
    * `host 192.168.1.1` = `192.168.1.1 0.0.0.0` (Egyetlen gép).
    * `any` = `0.0.0.0 255.255.255.255` (Bárki).

---

## 5. Modul: IPv4 ACL Konfigurálása

### 1. Standard ACL (Példa)
Cél: Engedjük a 192.168.10.0/24 hálózatot, de tiltsuk a 192.168.10.5-ös gépet.
```bash
access-list 10 deny host 192.168.10.5      <-- Specifikus szabály előre!
access-list 10 permit 192.168.10.0 0.0.0.255
!
interface g0/0
 ip access-group 10 out                    <-- Hozzárendelés interfészhez

```

### 2. Extended ACL (Példa)

Cél: A 192.168.10.0/24 hálózat elérhesse a webszervert (10.1.1.1), de semmi mást.

```bash
access-list 100 permit tcp 192.168.10.0 0.0.0.255 host 10.1.1.1 eq 80
! (Implicit deny a végén)
interface g0/0
 ip access-group 100 in                    <-- Bejövő irány a router felé

```

### 3. Named (Nevesített) ACL

Rugalmasabb, könnyebb szerkeszteni.

```bash
ip access-list extended WEB-ACCESS
 permit tcp 192.168.10.0 0.0.0.255 host 10.1.1.1 eq 80
 deny ip any any

```

### 4. VTY (Telnet/SSH) Védelem

Cél: Csak az ADMIN-PC (192.168.10.10) érhesse el a routert távolról.

```bash
access-list 20 permit host 192.168.10.10
!
line vty 0 4
 access-class 20 in                        <-- Itt "access-class" a parancs!

```

### Ellenőrzés

* `show access-lists`: Megmutatja a létrehozott listákat és az illeszkedések számát (matches).
* `show ip interface [int]`: Megmutatja, melyik ACL van az interfészre csatolva.



## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik ACL típust kell a forráshoz legközelebb elhelyezni?**

* *Válasz: Extended (Kiterjesztett) ACL.*

**2. Mi a Wildcard maszkja a /24-es alhálózatnak (255.255.255.0)?**

* *Válasz: 0.0.0.255.*

**3. Melyik parancsrendszerrel lehet korlátozni a távoli (Telnet/SSH) hozzáférést a routerhez?**

* *Válasz: Standard ACL létrehozása, majd a `line vty` alatt az `access-class [szám] in` parancs kiadása.*

**4. Mi történik azzal a csomaggal, ami egyik szabályra sem illeszkedik az ACL-ben?**

* *Válasz: Eldobásra kerül (Implicit Deny).*

**5. Melyik kulcsszóval helyettesíthető a `0.0.0.0 255.255.255.255` wildcard maszk?**

* *Válasz: any.*

**6. Melyik a helyes sorrend az ACL szabályoknál?**

* *Válasz: A specifikusabb (szűkebb) szabályok legyenek felül, az általánosabbak (tágabbak) alul.*

**7. Melyik parancs mutatja meg, hogy hányszor illeszkedett forgalom egy adott ACL szabályra?**

* *Válasz: `show access-lists`.*

---

# CCNA 3: Modul 6-8 Összefoglaló (NAT, WAN és VPN)

Ez a szakasz a privát hálózatok internetre juttatását (NAT), a nagy távolságú hálózatokat (WAN) és a biztonságos távoli elérést (VPN) tárgyalja.

---

## 6. Modul: NAT (Hálózati Címfordítás)

### Miért kell NAT?
Az IPv4 címek elfogytak. A NAT lehetővé teszi, hogy sok belső (privát) IP-címmel rendelkező eszköz egyetlen (vagy kevés) publikus IP-címen keresztül érje el az internetet.

### Címtípusok (NAT Terminológia)
* **Inside Local:** A belső eszköz tényleges privát IP-címe (pl. 192.168.1.10).
* **Inside Global:** A publikus cím, ahogy az internet látja a belső eszközt (pl. 209.165.200.226).
* **Outside Local:** A külső cél címe, ahogy a belső hálózat látja.
* **Outside Global:** A külső cél tényleges publikus IP-címe.

### NAT Típusok
1.  **Static NAT:** Egy-az-egyhez fordítás (1 privát -> 1 publikus). Szerverekhez használják, hogy kívülről mindig ugyanazon a címen érjék el.
2.  **Dynamic NAT:** Egy címkészletből (Pool) oszt ki publikus címet (First come, first served).
3.  **PAT (Port Address Translation) / NAT Overload:** Sok privát cím -> Egyetlen publikus cím. A **Forrás Portszám** alapján különbözteti meg a kapcsolatokat. Ez a leggyakoribb (pl. otthoni routerek).

### NAT Konfiguráció (Példa: PAT/Overload)
```bash
access-list 1 permit 192.168.1.0 0.0.0.255   <-- Kik használhatják a NAT-ot?
ip nat inside source list 1 interface g0/0/1 overload  <-- Fordítás beállítása
!
interface g0/0/0 (LAN)
 ip nat inside
!
interface g0/0/1 (WAN/Internet)
 ip nat outside

```

### NAT64

Átmeneti technológia, ami lehetővé teszi az IPv6-csak eszközök számára, hogy elérjék az IPv4 szervereket.

---

## 7. Modul: WAN (Wide Area Network) Alapok

### WAN Csatlakozások

* **Leased Line (Bérelt vonal):** Dedikált, pont-pont kapcsolat (pl. T1/E1). Drága, de garantált sávszélesség.
* **Circuit-Switched (Vonalkapcsolt):** PSTN, ISDN.
* **Packet-Switched (Csomagkapcsolt):** Frame Relay, ATM, **MPLS** (Multiprotocol Label Switching - a modern szolgáltatói szabvány), **Metro Ethernet**.

### Internet-alapú WAN

* **DSL (Digital Subscriber Line):** Telefonvonalon (rézérpár). ADSL (aszimmetrikus) a leggyakoribb.
* **Cable:** KábelTV hálózaton (koax/optika hibrid - HFC). Megosztott sávszélesség a szomszédokkal.
* **Fiber (FTTx):** Üvegszál az otthonig (FTTH).
* **Wireless:** 4G/5G, Műhold.

---

## 8. Modul: VPN (Virtuális Magánhálózat)

### Mi a VPN?

Biztonságos, titkosított csatorna (alagút/tunnel) egy nem biztonságos hálózaton (Internet) keresztül.

* **Előnyei:** Költséghatékony, Biztonságos, Skálázható.

### VPN Típusok

1. **Site-to-Site VPN:** Két telephely (router/tűzfal) állandó összekötése. A végfelhasználók észre sem veszik.
2. **Remote Access VPN:** Távmunkások (kliens szoftverrel) csatlakoznak a központi átjáróhoz.

### IPsec (Internet Protocol Security)

A legelterjedtebb VPN protokollcsomag.

* **CIA Triád biztosítása:**
* *Confidentiality (Titkosság):* Titkosítás (Encryption) - pl. **AES** (Advanced Encryption Standard).
* *Integrity (Integritás):* Hash algoritmusok - pl. **SHA** (Secure Hash Algorithm), MD5 (elavult).
* *Authentication (Hitelesítés):* **PSK** (Pre-Shared Key) vagy **RSA** digitális aláírás.
* *Diffie-Hellman (DH):* Biztonságos kulcscsere nyilvános csatornán.



### Egyéb VPN Technológiák

* **GRE (Generic Routing Encapsulation):** Alagút protokoll, de **NEM titkosít**! (Multicast routinghoz jó).
* **SSL/TLS VPN:** Webböngésző alapú (kliensmentes) VPN.
* **DMVPN (Dynamic Multipoint VPN):** Cisco megoldás, dinamikusan épít alagutakat telephelyek között (Hub-and-Spoke).



## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik parancs mutatja meg a NAT fordítási táblázatot?**

* *Válasz: `show ip nat translations`.*

**2. Melyik kulcsszó teszi lehetővé, hogy több belső host osztozzon egyetlen publikus IP-címen?**

* *Válasz: `overload`.*

**3. Mi a különbség a Site-to-Site és a Remote Access VPN között?**

* *Válasz: A Site-to-Site teljes hálózatokat köt össze (fix), a Remote Access egyedi felhasználókat (dinamikus).*

**4. Melyik algoritmus biztosítja az adatok titkosítását (Confidentiality) az IPsec-ben?**

* *Válasz: AES (vagy régebben 3DES).*

**5. Melyik algoritmus felelős az adatintegritásért (hogy nem módosult-e az adat)?**

* *Válasz: SHA (vagy HMAC).*

**6. Mi a "Demarcation Point" (Demarkációs pont) a WAN hálózatokban?**

* *Válasz: A határvonal a szolgáltató felelősségi köre és az ügyfél belső hálózata között.*

**7. Melyik WAN technológia használ címkéket (Labels) a forgalom gyorsítására?**

* *Válasz: MPLS (Multiprotocol Label Switching).*

---

# CCNA 3: Modul 9-12 Összefoglaló (QoS, Menedzsment, Tervezés, Hibaelhárítás)

Ez a szakasz a hálózati forgalom minőségének biztosítását (QoS), a hálózat felügyeletét (SNMP, Syslog, NTP), a tervezési elveket és a hibaelhárítást tárgyalja.

---

## 9. Modul: QoS (Quality of Service - Szolgáltatásminőség)

### Miért kell QoS?
A hang (VoIP) és videó forgalom érzékeny a késleltetésre és a csomagvesztésre. A QoS biztosítja, hogy ezek a fontos csomagok elsőbbséget élvezzenek a torlódás (Congestion) esetén.

### Hálózati jellemzők
* **Bandwidth (Sávszélesség):** Az átviteli kapacitás.
* **Congestion (Torlódás):** Amikor több adat jön, mint amennyit az interfész továbbítani tud.
* **Delay (Késleltetés / Latency):** A csomag útja a forrástól a célig. (VoIP max: 150 ms).
* **Jitter:** A késleltetés ingadozása. (VoIP max: 30 ms).
* **Packet Loss (Csomagvesztés):** Ha megtelik a buffer, a router eldobja a csomagokat. (VoIP max: 1%).

### Várakoztatási Algoritmusok (Queuing)
* **FIFO (First-In, First-Out):** Alapértelmezett. Nincs prioritás, érkezési sorrend.
* **WFQ (Weighted Fair Queuing):** Automatikus súlyozás, az alacsony sávszélességű forgalomnak (pl. Telnet) ad előnyt a nagy fájlokkal szemben.
* **CBWFQ (Class-Based WFQ):** Felhasználó által definiált osztályok (pl. Web, Mail) garantált sávszélességgel.
* **LLQ (Low Latency Queuing):** CBWFQ + egy szigorú prioritású sor (Strict Priority Queue) a hangnak (Voice). Ez a Cisco ajánlása VoIP-hoz.

### QoS Modellek
* **Best-Effort:** Nincs QoS (pl. Internet).
* **IntServ (Integrated Services):** Erőforrás-foglalás minden folyamatra (RSVP protokoll). Nem skálázható.
* **DiffServ (Differentiated Services):** A csomagokat osztályozzuk és megjelöljük (Marking). Skálázható, ez a mai szabvány.

### Jelölés (Marking)
* **L2:** CoS (Class of Service) mező a 802.1Q fejlécben (0-7). Hang: CoS 5.
* **L3:** DSCP (Differentiated Services Code Point) az IP fejlécben (0-63). Hang: EF (Expedited Forwarding - DSCP 46).

---

## 10. Modul: Hálózatfelügyelet (Network Management)

### CDP és LLDP (Szomszédfelderítés)
* **CDP (Cisco Discovery Protocol):** Cisco saját. Csak Cisco eszközöket lát.
* **LLDP (Link Layer Discovery Protocol):** Nyílt szabvány. Bármilyen gyártó eszközét látja.
* *Parancsok:* `show cdp neighbors (detail)`, `show lldp neighbors (detail)`.

### NTP (Network Time Protocol)
Időszinkronizálás. Fontos a logfájlok (naplók) időbélyege miatt, hogy tudjuk, mi mikor történt.
* **Stratum:** Távolság a pontos időforrástól (Stratum 0 = atomóra).
* *Parancs:* `ntp server [IP]`.

### SNMP (Simple Network Management Protocol)
Hálózati eszközök monitorozása és menedzselése.
* **Agent:** A routeren/switchen futó szoftver.
* **Manager (NMS):** A központi felügyeleti szerver.
* **MIB (Management Information Base):** Adatbázis a lekérdezhető változókról.
* **Üzenetek:** *Get* (adatkérés), *Set* (beállítás), *Trap* (riasztás küldése az Agenttől a Managernek).
* **Verziók:**
    * *SNMPv1/v2c:* Nem biztonságos (Community String jelszó szövegesen megy).
    * *SNMPv3:* Biztonságos (Hitelesítés + Titkosítás).

### Syslog
Központi naplózás. A routerek elküldik a log üzeneteket egy Syslog szerverre (UDP 514).
* **Severity Levels (Súlyosság):** 0 (Emergency) -> 7 (Debug). Minél kisebb a szám, annál súlyosabb a hiba.

---

## 11. Modul: Hálózattervezés

### Hierarchikus Modell
1.  **Access Layer (Hozzáférési réteg):** Végfelhasználók csatlakozása (L2 switchek, Port Security, VLAN-ok).
2.  **Distribution Layer (Elosztási réteg):** Házirendek, Routing, ACL-ek, redundancia.
3.  **Core Layer (Gerincréteg):** Gyors adattovábbítás (High-speed switching), megbízhatóság.

### Skálázhatóság
A hálózat növekedésének támogatása. Eszközök: Moduláris eszközök, EtherChannel, OSPF (hierarchikus routing).

---

## 12. Modul: Hibaelhárítás (Troubleshooting)

### Módszerek
* **Bottom-Up:** Fizikai rétegtől felfelé (Kábel -> Switch -> IP).
* **Top-Down:** Alkalmazástól lefelé (Böngésző -> IP -> Kábel).
* **Divide-and-Conquer (Oszd meg és uralkodj):** Pingeléssel (L3) kezdünk. Ha megy, a hiba feljebb van. Ha nem, lejjebb.

### IP Hibaelhárítás lépései
1.  **Fizikai:** Kábel, Interfész (Up/Up?).
2.  **Duplex:** Mismatch (Half/Full) -> Collisions.
3.  **L2:** VLAN, MAC címek.
4.  **Gateway:** Van-e beállítva Default Gateway a PC-n?
5.  **Routing:** Ismeri-e a router az utat? (`show ip route`).
6.  **Transport/ACL:** Tiltja-e valami a portot?
7.  **DNS:** Névfeloldás működik-e?



## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik QoS várakoztatási módszert (Queuing) ajánlja a Cisco a VoIP forgalomhoz?**
* *Válasz: LLQ (Low Latency Queuing).*

**2. Mi a Jitter?**
* *Válasz: A csomagok késleltetésének ingadozása (nem egyenletesen érkeznek).*

**3. Melyik SNMP verzió támogatja a titkosítást és a hitelesítést?**
* *Válasz: SNMPv3.*

**4. Melyik Syslog szint a legsúlyosabb (System Unusable)?**
* *Válasz: Level 0 (Emergency).*

**5. Melyik parancs mutatja meg a nem-Cisco szomszédos eszközöket is?**
* *Válasz: `show lldp neighbors`.*

**6. Mi a "Trap" üzenet az SNMP-ben?**
* *Válasz: Az Agent (eszköz) küldi a Managernek (szerver), ha valami rendkívüli esemény történik (pl. interfész leáll).*

**7. Melyik réteg feladata a gyors gerinc-összeköttetés biztosítása a hierarchikus modellben?**
* *Válasz: Core Layer.*

---

# CCNA 3: Modul 13-14 Összefoglaló (Virtualizáció és Automatizáció)

Ez a szakasz a modern hálózati trendeket: a felhőt, a szoftvervezérelt hálózatokat (SDN) és a programozható hálózatokat (API, JSON, Ansible) tárgyalja.

---

## 13. Modul: Hálózatvirtualizáció (Network Virtualization)

### Felhőalapú Számítástechnika (Cloud Computing)
* **Szolgáltatási Modellek:**
    * **SaaS (Software as a Service):** A szoftvert használjuk (pl. Office 365, Gmail).
    * **PaaS (Platform as a Service):** Fejlesztői környezet (pl. Google App Engine).
    * **IaaS (Infrastructure as a Service):** A vasat/hálózatot béreljük (pl. Amazon AWS, Cisco VACS).
* **Telepítési Modellek:** Public (Nyilvános), Private (Privát), Hybrid (Hibrid).

### Virtualizáció
Az operációs rendszer elválasztása a hardvertől.
* **Hypervisor:** A szoftver, ami a virtuális gépeket (VM) kezeli.
    * **Type 1 (Bare Metal):** Közvetlenül a hardveren fut (pl. VMware ESXi). Adatközpontokban ez a szabvány.
    * **Type 2 (Hosted):** Egy meglévő OS-en fut (pl. VirtualBox, VMware Workstation).

### SDN (Software-Defined Networking) - Szoftveralapú Hálózatok
A hagyományos hálózatban minden eszköznek saját "agya" van. Az SDN ezt szétválasztja:
1.  **Control Plane (Vezérlési sík):** Az "agy" (Routing döntések, ARP, STP). Az SDN-ben ezt egy **központi kontrollerre** (pl. Cisco DNA Center, APIC) helyezik át.
2.  **Data Plane (Adatsík):** Az "izom" (Csomagtovábbítás). Ez marad a fizikai eszközön.
3.  **Management Plane:** Konfiguráció és felügyelet (SSH, SNMP).

### Cisco ACI (Application Centric Infrastructure)
* **Topológia:** **Spine-Leaf** (Gerinc-Levél). Minden Leaf kapcsolódik minden Spine-hoz (Full Mesh), de Spine-ok nem kapcsolódnak egymáshoz.
* **Kontroller:** **APIC** (Application Policy Infrastructure Controller).

---

## 14. Modul: Hálózatautomatizálás (Network Automation)

### Adatformátumok (Data Formats)
Hogyan kommunikálnak a gépek egymással?
* **JSON (JavaScript Object Notation):** Emberi szemmel is olvasható, kulcs-érték párok. `{ "kulcs": "érték" }`. (A leggyakoribb).
* **XML (eXtensible Markup Language):** Tag-eket használ, mint a HTML. `<kulcs>érték</kulcs>`.
* **YAML (YAML Ain't Markup Language):** Behúzásokat (indentálást) használ, nagyon tiszta. (Ansible ezt használja).

### API (Application Programming Interface)
Szoftverek közötti kommunikációs híd.
* **REST API:** A legelterjedtebb webes API. HTTP metódusokat használ (mint a böngészők):
    * **GET:** Adat olvasása (**R**ead).
    * **POST:** Adat létrehozása (**C**reate).
    * **PUT/PATCH:** Adat módosítása (**U**pdate).
    * **DELETE:** Adat törlése (**D**elete).
    * *Ez a CRUD modell.*

### Konfigurációkezelő Eszközök (Config Management Tools)
Automatizálják a hálózati eszközök beállítását (több száz routert egyszerre).

| Jellemző | **Ansible** | **Puppet** | **Chef** |
| :--- | :--- | :--- | :--- |
| **Nyelv** | Python | Ruby | Ruby |
| **Agent** | **Agentless** (Nem kell kliens) | Agent-based | Agent-based |
| **Működés** | **Push** (Kiküldi a configot) | Pull (Eszköz letölti) | Pull |
| **Fájlformátum** | **YAML** (Playbooks) | Manifest | Cookbook |

### Cisco DNA Center és IBN (Intent-Based Networking)
Szándékalapú hálózat. Nem parancsokat írunk ("legyen VLAN 10"), hanem célt adunk meg ("A vendégek érjék el a netet").
* **Underlay:** A fizikai hálózat (kábelek, IP-k).
* **Overlay:** A logikai hálózat (VXLAN alagutak), ezen megy a forgalom.



## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik Hypervisor típus fut közvetlenül a hardveren (operációs rendszer nélkül)?**
* *Válasz: Type 1 (Bare Metal).*

**2. Melyik felhőszolgáltatási modell biztosít hálózati hardvert és infrastruktúrát bérlésre?**
* *Válasz: IaaS (Infrastructure as a Service).*

**3. Melyik sík felelős a csomagtovábbításért (forwarding) az SDN architektúrában?**
* *Válasz: Data Plane (Adatsík).*

**4. Melyik HTTP metódust használja a REST API új adat létrehozására?**
* *Válasz: POST.*

**5. Melyik konfigurációkezelő eszköz "Agentless" (ügynök nélküli) és használ YAML formátumot?**
* *Válasz: Ansible.*

**6. Milyen zárójelet használ a JSON az objektumok (kulcs-érték párok) jelölésére?**
* *Válasz: Kapcsos zárójel `{ }`.*

**7. Mi a "Spine-Leaf" topológia jellemzője a Cisco ACI-ban?**
* *Válasz: Minden Leaf switch csatlakozik minden Spine switch-hez, de a Spine-ok nem csatlakoznak egymáshoz.*

---

