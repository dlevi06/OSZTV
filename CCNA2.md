# CCNA 2: Modul 1-4 Összefoglaló (Switching, VLAN, Inter-VLAN Routing)

Ez a szakasz a kapcsolók (switchek) konfigurálását, a VLAN-ok kialakítását és a VLAN-ok közötti kommunikációt (Router-on-a-Stick, L3 Switch) tárgyalja. Az OSZTV-n ez a gyakorlati feladatok (Packet Tracer) gerince.

---

## 1. Modul: Switch Alapbeállítások

### A Switch indítása (Boot Sequence)
1.  **POST:** Hardverteszt.
2.  **Boot Loader:** Betöltő program.
3.  **IOS Betöltése:** Flash memóriából. (Beállítható: `boot system flash:/image.bin`).
4.  **Konfiguráció:** `startup-config` (NVRAM) -> `running-config` (RAM).

### Alapvető Konfiguráció
* **SVI (Switch Virtual Interface):** Menedzsment IP-cím beállítása (hogy távolról elérjük a switchet). Alapértelmezésben VLAN 1.
    ```bash
    Switch(config)# interface vlan 1
    Switch(config-if)# ip address 192.168.1.2 255.255.255.0
    Switch(config-if)# no shutdown
    Switch(config)# ip default-gateway 192.168.1.1  <-- Hogy más hálózatból is elérjük
    ```

### Duplex és Sebesség
* **Full-Duplex:** Egyszerre ad és vesz. Nincs ütközés (Collision).
* **Half-Duplex:** Egyszerre csak egy irány. Ütközések lehetségesek.
* **Auto-MDIX:** Automatikusan felismeri, hogy egyenes (straight) vagy kereszt (crossover) kábel van-e bedugva.

### Biztonságos távoli elérés (SSH) - OSZTV KRITIKUS!
A Telnet nem biztonságos (szövegesen küldi a jelszót). Helyette **SSH**-t használunk (22-es port).
**Beállítás lépései:**
1.  `hostname S1` (Név kötelező!)
2.  `ip domain-name cisco.com` (Domain név kötelező a kulcshoz)
3.  `crypto key generate rsa` (Kulcs generálása)
4.  `username admin secret jelszo` (Helyi felhasználó)
5.  `line vty 0 4` -> `transport input ssh` és `login local`

---

## 2. Modul: Switching Koncepciók

### Kerettovábbítás (Frame Forwarding)
A switch a **MAC-címtáblát (CAM table)** használja a döntéshez.
1.  **Tanulás (Learn):** A bejövő keret **Forrás MAC** címét eltárolja a porttal együtt.
2.  **Továbbítás (Forward):** A **Cél MAC** alapján küldi a megfelelő portra.
3.  **Flooding (Elárasztás):** Ha a cél MAC ismeretlen (vagy Broadcast), kiküldi minden porton (kivéve a bejövőn).

### Kapcsolási módok
* **Store-and-Forward:** Beolvassa az egész keretet, ellenőrzi a hibákat (CRC/FCS), és csak a jókat továbbítja. (Megbízható).
* **Cut-Through:**
    * *Fast-Forward:* Azonnal továbbít, amint megvan a Cél MAC. (Leggyorsabb, de hibásat is továbbíthat).
    * *Fragment-Free:* Az első 64 bájtot (ütközési ablak) olvassa be.

### Tartományok (Domains)
* **Collision Domain (Ütközési):** Hubnál az egész hálózat egy. Switch esetében **minden port külön ütközési tartomány** (ez a jó!).
* **Broadcast Domain (Szórási):** Amíg eljut a Broadcast üzenet. Alapból egy Switch = 1 Broadcast Domain. Csak a **Router** (vagy VLAN) választja szét őket.

---

## 3. Modul: VLAN-ok (Virtuális LAN-ok)

### Miért jó?
Logikailag különválasztja az eszközöket, csökkenti a Broadcast forgalmat, növeli a biztonságot.
**1 VLAN = 1 Broadcast Domain = 1 Alhálózat.**

### VLAN Típusok
* **Default VLAN:** VLAN 1. Minden port alapból itt van.
* **Data VLAN:** Felhasználói adatoknak.
* **Voice VLAN:** IP telefonoknak (magasabb prioritás/QoS kell neki).
* **Native VLAN:** A **Trunk** kapcsolaton a **címkézetlen (untagged)** keretek ide tartoznak. Biztonsági okból érdemes megváltoztatni az alapértelmezettről (pl. VLAN 99).

### Trönk (Trunk) - 802.1Q
Két switch (vagy switch és router) közötti kapcsolat, ami **több VLAN forgalmát** viszi.
* A kereteket megjelöli (Tagging) a VLAN ID-val (802.1Q szabvány).
* **DTP (Dynamic Trunking Protocol):** Cisco protokoll a trunk automatikus egyeztetésére.
    * *Dynamic Desirable:* Akar trunk lenni.
    * *Dynamic Auto:* Várja, hogy a másik oldal akar-e.

### Konfiguráció (Példa)
```bash
vlan 10
name SALES
interface fa0/1
 switchport mode access
 switchport access vlan 10
interface gi0/1
 switchport mode trunk
 switchport trunk native vlan 99

```

---

## 4. Modul: Inter-VLAN Routing (VLAN-ok közötti forgalom)

Mivel a VLAN-ok el vannak szeparálva, kell egy Layer 3 eszköz (Router vagy L3 Switch), hogy kommunikáljanak.

### 1. Router-on-a-Stick (ROAS) - Leggyakoribb OSZTV feladat!

Egy router fizikai interfészét több virtuális **alinterfészre (subinterface)** bontjuk.

**Konfiguráció (Router):**

```bash
interface g0/0.10
 encapsulation dot1Q 10   <-- Fontos: VLAN ID megadása!
 ip address 192.168.10.1 255.255.255.0
interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
interface g0/0
 no shutdown              <-- A fizikai portot kell bekapcsolni!

```

*A Switchen a router felé menő portnak **TRUNK**-nak kell lennie!*

### 2. Layer 3 Switch (SVI routing)

Ha a switch tud routingot (pl. Cisco Catalyst 2960 nem, de 3650 igen).

* Létrehozunk SVI-ket (VLAN interfészeket) IP-címmel.
* Kiadjuk az `ip routing` parancsot.

### Hibaelhárítás

* **Native VLAN Mismatch:** A trunk két végén eltér a Native VLAN -> Hibaüzenet a konzolon, adatvesztés.
* **Hiányzó VLAN:** Ha a VLAN nincs létrehozva a switchen, a trunk nem továbbítja.
* **Rossz IP/Maszk:** A PC Gateway-e nem a router alinterfészének címe.

---

## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik parancs mutatja meg a switch portjainak állapotát és a VLAN hozzárendelést?**

* *Válasz: `show vlan brief` vagy `show interfaces switchport`.*

**2. Mi a Native VLAN jellemzője a 802.1Q trönkön?**

* *Válasz: A Native VLAN forgalma **nincs megcímkézve (untagged)**.*

**3. Melyik kapcsolási mód (switching method) ellenőrzi a keret hibáit (FCS) továbbítás előtt?**

* *Válasz: Store-and-Forward.*

**4. Mi a teendő, ha a Router-on-a-Stick konfigurációnál a PC-k nem érik el egymást?**

* *Válasz: Ellenőrizni, hogy a switchen a router felé menő port **TRUNK** módban van-e, és a routeren az `encapsulation dot1q [vlan_id]` parancs helyes-e.*

**5. Melyik parancsra van szükség a router alinterfészén a VLAN azonosításához?**

* *Válasz: `encapsulation dot1Q [vlan_szám]`.*

**6. Mi történik, ha egy switch keretet kap, de a cél MAC cím nincs a táblájában?**

* *Válasz: Elárasztja (Flooding) az összes porton, kivéve a bejövőt.*

**7. Melyik parancs titkosítja a jelszavakat a konfigurációs fájlban (pl. a `show running-config` kimenetben)?**

* *Válasz: `service password-encryption`.*

**8. Mi a legbiztonságosabb módja a switch távoli menedzselésének?**

* *Válasz: SSH (Secure Shell), mert titkosítja a forgalmat (a Telnettel ellentétben).*

---

# CCNA 2: Modul 5-6 Összefoglaló (STP és EtherChannel)

Ez a szakasz a redundáns hálózatok működését tárgyalja. Az STP megakadályozza a hurkokat, az EtherChannel pedig növeli a sávszélességet és a megbízhatóságot.

---

## 5. Modul: STP (Spanning Tree Protocol)

### A probléma: L2 Hurkok (Loops)
Redundáns (tartalék) kábelezés esetén a switchek között hurkok alakulhatnak ki.
* **Broadcast Storm (Szórási vihar):** A Broadcast keretek végtelenül keringenek, megbénítva a hálózatot.
* **MAC tábla instabilitás:** A switch folyamatosan átírja a MAC címek portjait.

### Az STP Működése (IEEE 802.1D)
Az STP logikailag lezár (Block) bizonyos portokat, hogy megszüntesse a hurkot, de ha szakadás van, újra kinyitja őket (Redundancia).
**BPDU (Bridge Protocol Data Unit):** Ezekkel az üzenetekkel kommunikálnak a switchek.

### STP Szerepkörök (Election Process)
1.  **Root Bridge (Gyökérhíd):** A főnök. Mindenki hozzá igazodik.
    * Kiválasztás: A **legalacsonyabb Bridge ID (BID)** nyer.
    * **BID = Priority + MAC cím.** (Alapértelmezett Priority: 32768).
2.  **Port Szerepek:**
    * **Root Port (RP):** A legrövidebb út a Root Bridge felé (minden nem-Root switchen 1 db).
    * **Designated Port (DP):** Továbbító port a szegmensen (a Root Bridge minden portja DP).
    * **Alternate / Blocked Port (AP):** Lezárt port a hurok elkerülésére (tartalék).

### STP Változatok
* **PVST+ (Per-VLAN Spanning Tree):** Cisco alapértelmezés. Minden VLAN-nak külön STP példánya van (terheléselosztás lehetséges).
* **RSTP (Rapid STP - 802.1w):** Sokkal gyorsabb konvergencia (másodpercek alatt áll helyre).
    * Port állapotok: *Discarding, Learning, Forwarding*.

### Fontos Kiegészítők
* **PortFast:** Azonnal *Forwarding* állapotba teszi a portot (csak végpontokhoz/PC-khez szabad használni!).
* **BPDU Guard:** Ha BPDU-t (switch üzenetet) kap egy PortFast porton, azonnal letiltja a portot (biztonság).

---

## 6. Modul: EtherChannel (Link Aggregation)

### Mi az EtherChannel?
Több fizikai kábelt (linket) egyetlen logikai csatornába (Port-Channel) fog össze.
* **Előnyök:**
    * **Nagyobb sávszélesség:** Pl. 4 db 1 Gbps link = 4 Gbps logikai link.
    * **Redundancia:** Ha egy kábel elszakad, a többi viszi tovább a forgalmat (STP nem számolja újra).
    * **Terheléselosztás (Load Balancing).**

### Protokollok
1.  **PAgP (Port Aggregation Protocol):** Cisco saját.
    * Módok: *Auto / Desirable*.
2.  **LACP (Link Aggregation Control Protocol - 802.3ad):** Nyílt szabvány (mindenki ezt használja).
    * Módok: *Passive / Active*.
3.  **Static (ON):** Nincs protokoll, kézzel kényszerítve.

### Konfigurációs Szabályok
* Minden portnak **azonosnak** kell lennie:
    * Sebesség (Speed)
    * Duplex mód
    * VLAN (Native és Allowed)
    * Trunk/Access mód

### Konfiguráció (LACP Példa)
```bash
interface range g0/1-2
 channel-group 1 mode active  <-- Létrehozza a Port-Channel 1-et
 no shutdown

interface port-channel 1      <-- Ezt kell konfigurálni (Trunk, VLAN)!
 switchport mode trunk
 switchport trunk allowed vlan 10,20

```

---

## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik switch lesz a Root Bridge?**

* *Válasz: Amelyiknek a legkisebb a Bridge ID-ja (Priority + MAC).*

**2. Mi a PortFast feladata?**

* *Válasz: A végpontokhoz (PC) csatlakozó portokon kikerüli az STP várakozási idejét, és azonnal továbbít.*

**3. Melyik EtherChannel módok alkotnak kapcsolatot LACP esetén?**

* *Válasz: Active - Active VAGY Active - Passive. (Passive - Passive NEM működik).*

**4. Mi történik, ha az EtherChannel egyik fizikai linkje megszakad?**

* *Válasz: A kapcsolat nem szakad meg, a forgalom a maradék linkeken megy tovább (nincs STP újraszámolás).*

**5. Milyen típusú port az "Alternate Port" az STP-ben?**

* *Válasz: Lezárt (Blocking/Discarding) port, ami megelőzi a hurkot.*

**6. Mi a feltétele az EtherChannel létrehozásának?**

* *Válasz: A portok sebességének, duplex módjának és VLAN beállításainak egyezniük kell.*

**7. Melyik parancs mutatja meg az EtherChannel állapotát?**

* *Válasz: `show etherchannel summary`.*

---

# CCNA 2: Modul 7-9 Összefoglaló (DHCP, IPv6 Címkiosztás, FHRP)

Ez a szakasz a dinamikus IP-cím kiosztást (IPv4 és IPv6) és az alapértelmezett átjáró redundanciáját (HSRP) tárgyalja.

---

## 7. Modul: DHCPv4 (Dynamic Host Configuration Protocol)

### Működése (DORA folyamat)
1.  **D**iscover: A kliens keresi a szervert (Broadcast: 255.255.255.255).
2.  **O**ffer: A szerver ajánlatot tesz egy IP-címre (Unicast vagy Broadcast).
3.  **R**equest: A kliens elfogadja az ajánlatot (Broadcast, hogy a többi szerver is lássa).
4.  **A**ck (Acknowledgment): A szerver véglegesíti a bérletet (Lease).

### Router konfigurálása DHCP szerverként
```bash
ip dhcp excluded-address 192.168.1.1 192.168.1.10  <-- Kizárt címek (statikus)
ip dhcp pool LAN-POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8

```

### DHCP Relay (IP Helper)

Ha a DHCP szerver másik hálózatban van (a router mögött), a router alapból nem engedi át a Broadcast kérést.
Megoldás: **IP Helper Address**.

```bash
interface g0/0/1 (A kliens felőli interfész!)
 ip helper-address 10.1.1.2 (A szerver IP címe)

```

---

## 8. Modul: SLAAC és DHCPv6

IPv6-nál a kliens háromféleképpen kaphat címet. Ezt a Router Advertisement (RA) üzenet **Flag**-jei döntik el.

### 1. SLAAC (Stateless Address Auto-Configuration)

* **Működés:** A router megadja a prefixet, a kliens generálja a saját Interface ID-ját (EUI-64 vagy Random).
* **Flag:** A=1, M=0, O=0.
* **Nincs DHCP szerver.**

### 2. Stateless DHCPv6 (SLAAC + DHCP)

* **Működés:** A címet a SLAAC adja, de az egyéb infókat (DNS) a DHCP szerver.
* **Flag:** A=1, M=0, **O=1** (Other info).

### 3. Stateful DHCPv6

* **Működés:** Mindent a DHCP szerver ad (mint IPv4-nél).
* **Flag:** A=0, **M=1** (Managed), O=0.

### EUI-64

A 48 bites MAC címből generál 64 bites Interface ID-t úgy, hogy a közepére beszúrja az **FFFE** értéket, és a 7. bitet megfordítja.

---

## 9. Modul: FHRP (First Hop Redundancy Protocols)

### Mi a probléma?

Ha a PC alapértelmezett átjárója (Default Gateway) egyetlen router, és az meghibásodik, a PC nem éri el az internetet.

### Megoldás: Virtuális Router

Több fizikai router (pl. R1 és R2) egyetlen **virtuális routernek** látszik, közös **Virtuális IP-vel** és **Virtuális MAC-címmel**. A PC ezt a virtuális IP-t használja Gateway-ként.

### Protokollok

1. **HSRP (Hot Standby Router Protocol):** Cisco saját.
* **Active Router:** Továbbítja a forgalmat.
* **Standby Router:** Figyel, és ha az Active kiesik, átveszi a helyét.
* *Verziók:* HSRPv1, HSRPv2.


2. **VRRP (Virtual Router Redundancy Protocol):** Nyílt szabvány.
3. **GLBP (Gateway Load Balancing Protocol):** Cisco saját, terheléselosztást is tud (több router is aktív egyszerre).

### HSRP Konfiguráció (Példa)

```bash
interface g0/1
 standby 1 ip 192.168.1.254     <-- Virtuális IP
 standby 1 priority 150         <-- Nagyobb prioritás = Active (Alap: 100)
 standby 1 preempt              <-- Visszaveszi a szerepet, ha újraindul

```

---

## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik DHCP üzenet az első, amit a kliens küld?**

* *Válasz: DHCPDISCOVER (Broadcast).*

**2. Melyik parancs teszi lehetővé, hogy a router továbbítsa a DHCP kéréseket egy távoli szervernek?**

* *Válasz: `ip helper-address [szerver_ip]`.*

**3. Melyik IPv6 címkiosztási módszernél generálja a kliens a saját IP-címét a router prefixe alapján?**

* *Válasz: SLAAC (Stateless Address Auto-Configuration).*

**4. Mi a HSRP (Hot Standby Router Protocol) célja?**

* *Válasz: Az alapértelmezett átjáró redundanciájának biztosítása (ha egy router kiesik, a másik átveszi).*

**5. Melyik router lesz az Active router HSRP esetén?**

* *Válasz: Amelyiknek a legmagasabb a prioritása (Priority). Ha egyenlő, akkor a legmagasabb IP-című.*

**6. Hogyan működik az EUI-64 az IPv6 cím generálásakor?**

* *Válasz: A MAC-cím közepére beszúrja az FFFE-t, és megfordítja a 7. bitet.*

**7. Melyik portokat használja a DHCPv4?**

* *Válasz: UDP 67 (Szerver) és UDP 68 (Kliens).*

---

# CCNA 2: Modul 10-13 Összefoglaló (LAN Security és WLAN)

Ez a szakasz a vezetékes hálózatok (LAN) védelmét, a támadástípusokat, valamint a vezeték nélküli hálózatok (WLAN) alapjait és konfigurálását tárgyalja. Az OSZTV-n a biztonsági beállítások (Port Security, DAI, DHCP Snooping) és a Wi-Fi szabványok gyakori témák.

---

## 10. Modul: LAN Security Concepts

### Végponti Biztonság (Endpoint Security)
A végpontok (PC, laptop) a legsebezhetőbbek.
* **Védelem:** Vírusirtó, Tűzfal, **Cisco ESA** (Email Security Appliance - levélszemét/phishing ellen), **Cisco WSA** (Web Security Appliance - webes fenyegetések ellen).

### AAA Keretrendszer (Authentication, Authorization, Accounting)
1.  **Authentication (Hitelesítés):** *Ki vagy?* (Jelszó, tanúsítvány).
2.  **Authorization (Jogosultság):** *Mit tehetsz?* (Parancsok futtatása, hozzáférés).
3.  **Accounting (Naplózás):** *Mit tettél?* (Mikor léptél be, mit írtál be).
* **802.1X:** Port-alapú hitelesítés. Csak hitelesített eszközök (Supplicant) kapnak hálózati hozzáférést a switch porton (Authenticator) keresztül, egy RADIUS szerver (Authentication Server) segítségével.

### Layer 2 Támadások
* **MAC Address Table Flooding:** A támadó hamis MAC címekkel árasztja el a switchet, hogy megteljen a tábla. Ekkor a switch "buta hub"-ként kezd viselkedni (mindenkinek elküldi a forgalmat).
* **VLAN Hopping:** A támadó átugrik egy másik (jogosulatlan) VLAN-ba (pl. DTP spoofinggal vagy Double Tagginggel).
* **DHCP Starvation:** A támadó az összes szabad IP-címet kikéri a DHCP szervertől (hamis MAC címekkel).
* **DHCP Spoofing:** A támadó elindít egy hamis DHCP szervert, és rossz átjárót (Gateway) ad a klienseknek (Man-in-the-Middle).
* **ARP Spoofing/Poisoning:** A támadó a saját MAC címét társítja az átjáró IP címéhez a kliens ARP táblájában.

---

## 11. Modul: Switch Security Configuration

### Port Security (MAC Flooding ellen)
Korlátozza, hány és milyen MAC cím csatlakozhat egy porthoz.
* **Beállítás:**
    ```bash
    switchport mode access
    switchport port-security
    switchport port-security maximum 2
    switchport port-security mac-address sticky  <-- Megtanulja és elmenti a MAC címet
    switchport port-security violation shutdown  <-- Ha szabálysértés van, lekapcsol
    ```
* **Violation Módok:**
    * *Shutdown:* Port lekapcsol (Err-disabled), naplóz. (Alapértelmezett).
    * *Restrict:* Eldobja a csomagot, naplóz.
    * *Protect:* Csak eldobja a csomagot (nem naplóz).

### DHCP Snooping (DHCP támadások ellen)
Megkülönbözteti a megbízható (Trusted - szerver felé) és nem megbízható (Untrusted - kliens felé) portokat. Csak a Trusted portról enged DHCP választ (Offer/Ack).
* **Beállítás:** `ip dhcp snooping`, `ip dhcp snooping vlan 10`, interfészen: `ip dhcp snooping trust`.

### Dynamic ARP Inspection (DAI) (ARP támadások ellen)
A DHCP Snooping adatbázisát használja. Ellenőrzi, hogy az ARP kérésben lévő IP-MAC páros megegyezik-e a DHCP által kiosztottal.

---

## 12. Modul: WLAN Concepts (Vezeték nélküli alapok)

### 802.11 Szabványok
* **2.4 GHz:** Nagyobb hatótáv, de lassabb és zavartabb (mikró, Bluetooth). Csatornák: 1, 6, 11 (átfedésmentes).
* **5 GHz:** Gyorsabb, több csatorna, de kisebb hatótáv (falak jobban árnyékolják).
* **Szabványok:**
    * *802.11b/g:* 2.4 GHz.
    * *802.11a/ac:* 5 GHz.
    * *802.11n/ax (Wi-Fi 6):* Mindkettő (Dual Band).

### WLAN Komponensek
* **AP (Access Point):** Vezeték nélküli hozzáférési pont.
    * *Autonomous AP:* Önálló, egyenként kell konfigurálni.
    * *Lightweight AP (LAP):* "Buta" AP, egy központi vezérlő (**WLC** - Wireless LAN Controller) irányítja (CAPWAP protokollal).

### WLAN Biztonság
* **SSID Cloaking:** A hálózat nevének elrejtése (nem valódi védelem).
* **MAC Filtering:** Csak engedélyezett eszközök léphetnek be.
* **Titkosítás:**
    * *Open:* Nincs jelszó.
    * *WPA2 (Personal):* Jelszó (PSK - Pre-Shared Key). AES titkosítást használ.
    * *WPA2 (Enterprise):* 802.1X / RADIUS szerverrel hitelesít (felhasználónév + jelszó).

---

## 13. Modul: WLAN Configuration

### Konfiguráció (WLC - Wireless LAN Controller)
Nagyvállalati környezetben WLC-t használunk.
1.  **VLAN létrehozása:** A vezeték nélküli forgalomnak.
2.  **Interface létrehozása a WLC-n:** Összerendeli a VLAN-t, IP-címet és fizikai portot.
3.  **WLAN létrehozása:** SSID megadása, Interface kiválasztása, Biztonság (WPA2+AES+PSK vagy 802.1X) beállítása.

### CAPWAP és FlexConnect
* **CAPWAP:** Az AP és a WLC közötti kommunikációs alagút (Tunnel). Az adatforgalom ezen megy át.
* **FlexConnect:** Ha a WLC elérhetetlen (pl. távoli irodában megszakad a WAN), az AP önállóan is képes a helyi forgalmat kapcsolni (Local Switching).

---

## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik támadástípus célja a switch MAC-címtáblájának megtöltése?**
* *Válasz: MAC Address Flooding.*

**2. Melyik Port Security mód kapcsolja le a portot (Err-disabled) szabálysértés esetén?**
* *Válasz: Shutdown.*

**3. Melyik technológia védi a hálózatot a hamis DHCP szerverektől?**
* *Válasz: DHCP Snooping.*

**4. Melyik Wi-Fi frekvenciasávnak van több csatornája és kisebb hatótávolsága?**
* *Válasz: 5 GHz.*

**5. Melyik eszköz kezeli a Lightweight AP-kat (LAP) nagyvállalati környezetben?**
* *Válasz: WLC (Wireless LAN Controller).*

**6. Melyik hitelesítési mód igényel RADIUS szervert a Wi-Fi hálózaton?**
* *Válasz: WPA2 Enterprise (802.1X).*

**7. Mi a teendő, ha a Wi-Fi hálózat lassú a 2.4 GHz-es sáv zsúfoltsága miatt?**
* *Válasz: Át kell terelni a forgalmat (pl. videó streaming) az 5 GHz-es sávra (Dual-Band eszközökkel).*

---

# CCNA 2: Modul 14-16 Összefoglaló (Routing Concepts & Static Routing)

Ez a szakasz a routerek működését, a routing tábla felépítését és a statikus útvonalak konfigurálását/hibaelhárítását tárgyalja.

---

## 14. Modul: Routing Concepts (Forgalomirányítási Alapok)

### A Router Döntési Folyamata
A router három fő szempont alapján választ útvonalat:
1.  **Longest Match (Leghosszabb egyezés):** Ha több útvonal is illeszkedik a cél IP-címre, azt választja, amelyiknél a maszk hosszabb (több bit egyezik).
    * *Példa:* Cél: `192.168.10.10`.
    * Útvonal A: `192.168.10.0/24` (24 bit egyezik).
    * Útvonal B: `192.168.10.0/28` (28 bit egyezik) -> **Ez nyer!**
2.  **Administrative Distance (AD):** Az útvonal forrásának megbízhatósága. Kisebb = Jobb.
    * *Connected:* 0
    * *Static:* 1
    * *EIGRP:* 90
    * *OSPF:* 110
    * *RIP:* 120
3.  **Metric (Mérték):** Az útvonal "költsége" (pl. sávszélesség, ugrások száma). Kisebb = Jobb.

### Routing Tábla Kódok
* **C (Connected):** Közvetlenül csatlakozó hálózat.
* **L (Local):** A router saját interfészének IP-címe (/32).
* **S (Static):** Statikus útvonal.
* **S* (Default Static):** Alapértelmezett statikus útvonal (Gateway of Last Resort).
* **O (OSPF), D (EIGRP):** Dinamikus protokollok.

---

## 15. Modul: IP Static Routing (Statikus Útvonalak)

### Miért használjuk?
Kisebb hálózatokban, vagy Stub (zsákutca) hálózatoknál, ahol csak egy kijárat van. Biztonságosabb és kevesebb erőforrást igényel, mint a dinamikus routing, de nehezebb karbantartani.

### Típusok és Konfiguráció
**Parancs:** `ip route [cél_hálózat] [maszk] [következő_ugrás_IP vagy kimenő_interfész]`

1.  **Standard Static Route:**
    * *Next-Hop IP:* `ip route 192.168.2.0 255.255.255.0 10.0.0.2` (Rekurzív keresés).
    * *Directly Connected:* `ip route 192.168.2.0 255.255.255.0 g0/0/1` (Csak pont-pont kapcsolaton!).
    * *Fully Specified:* `ip route 192.168.2.0 255.255.255.0 g0/0/1 10.0.0.2` (Interfész ÉS Next-Hop IP).

2.  **Default Static Route (Alapértelmezett):**
    * Ha nincs találat a táblában, ide küldi. ("Minden más" forgalom, pl. Internet felé).
    * `ip route 0.0.0.0 0.0.0.0 10.0.0.2`

3.  **Floating Static Route (Lebegő):**
    * Tartalék útvonal. Csak akkor aktív, ha az elsődleges kiesik.
    * Magasabb AD-t kell megadni neki (pl. 5), mint az elsődlegesnek (pl. 1).
    * `ip route 0.0.0.0 0.0.0.0 10.0.0.5 5`

4.  **Host Route:**
    * Egyetlen konkrét géphez (/32) vezető út.
    * `ip route 192.168.10.10 255.255.255.255 10.0.0.2`

---

## 16. Modul: Troubleshooting Static Routes (Hibaelhárítás)

### Gyakori Hibák
1.  **Hibás Interfész/IP:** Rossz IP-cím vagy interfész lett megadva a parancsban.
2.  **Interfész Down:** Ha a kimenő interfész leáll, a hozzá tartozó statikus útvonal törlődik a táblából.
3.  **Hiányzó Visszaút:** A csomag eljut a célig, de a válasz nem tud visszajönni, mert a túloldali routernek nincs útvonala visszafelé.

### Ellenőrző Parancsok
* `ping` / `traceroute`: Elérhetőség tesztelése.
* `show ip route`: Routing tábla (látszik-e az 'S' bejegyzés?).
* `show ip route static`: Csak a statikus utak.
* `show ip interface brief`: Interfészek állapota (Up/Up?).
* `show cdp neighbors detail`: Szomszéd IP-címének ellenőrzése.

---

## OSZTV Kiemelt Ellenőrző Kérdések (a megadott anyagból)

**1. Melyik útvonalat választja a router, ha több is illeszkedik a célra?**
* *Válasz: A leghosszabb egyezést (Longest Match).*

**2. Mi az Administrative Distance (AD) szerepe?**
* *Válasz: Az útvonal forrásának megbízhatóságát jelzi. (Kisebb érték = megbízhatóbb).*

**3. Melyik parancs állít be alapértelmezett statikus útvonalat (Default Route)?**
* *Válasz: `ip route 0.0.0.0 0.0.0.0 [next-hop-ip]`.*

**4. Mi a Floating Static Route célja?**
* *Válasz: Tartalék útvonal biztosítása (csak akkor aktív, ha az elsődleges kiesik).*

**5. Hogyan lehet Floating Static Route-ot konfigurálni?**
* *Válasz: Meg kell adni egy magasabb Administrative Distance értéket a parancs végén (pl. `... 10.0.0.2 5`).*

**6. Mi történik a statikus útvonallal, ha a hozzá tartozó kimenő interfész "Down" állapotba kerül?**
* *Válasz: Törlődik a routing táblából.*

**7. Melyik routing tábla kód jelöli a statikus útvonalat?**
* *Válasz: S.*
