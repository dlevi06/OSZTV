# 1. Fejezet: Bevezetés a személyi számítógépek hardvereinek világába (ITE)

Ez a fejezet az OSZTV szempontjából alapozó jellegű, de a **biztonsági előírások**, a **feszültségszintek** és a **chipset felépítése** gyakran előkerülő kérdések.

---

## 1. Hardver Alapismeretek és Biztonság

### Elektromos biztonság és ESD (Elektrosztatikus kisülés)
A számítógép alkatrészei (főleg a RAM és a CPU) rendkívül érzékenyek a statikus elektromosságra. Már 30 volt is károsíthatja őket, de az ember csak kb. 3000 voltot érez meg.

* **ESD elleni védelem (Eszközök):**
    * **Antisztatikus csuklópánt:** A bőrhöz kell érnie, és a ház egy *festetlen* fémfelületéhez kell csíptetni.
    * **Antisztatikus alátét:** Erre helyezzük az alkatrészeket szerelés közben.
* **Földelés:** A kóbor áram elvezetésére szolgál (kisebb ellenállású utat biztosít), megvédve az embert és az eszközt.
* **Tápellátás védelme:**
    * **UPS (Szünetmentes tápegység):** Áramkimaradás esetén akkumulátorról táplálja a gépet.
    * **Túlfeszültségvédő:** Hirtelen feszültségtüskék (pl. villámcsapás hatása) ellen véd.

### Ház és Tápegység (PSU)
A ház kiválasztásánál a legfontosabb szempont az **alaplap formátuma** (Form Factor) és a **bővíthetőség**.

* **Tápegység funkciója:** A hálózati váltakozó áramot (AC) a számítógép számára szükséges egyenárammá (DC) alakítja.
* **Fontos feszültségszintek (OSZTV kérdés!):**
    * **3.3V és 5V:** Digitális áramkörök, alaplap, chipset, memória.
    * **12V:** Mozgó alkatrészek (HDD motorok, ventilátorok) és modern CPU-k/Videókártyák tápellátása.
* **Kiválasztás:** Mindig az alkatrészek összteljesítménye (Watt) alapján kell méretezni.

---

## 2. Az Alaplap és a Chipset (Kiemelt fontosságú!)

Az alaplap a rendszer központja. A rajta lévő **buszok** (vezetősávok) szállítják az adatokat. A vezérlést a **Chipset** végzi.

### A Chipset felépítése (Klasszikus architektúra):
1.  **Northbridge (Északi híd):**
    * A "gyors" kommunikációért felel.
    * Közvetlen kapcsolatot teremt a **CPU**, a **RAM** és a **Videókártya** (PCIe/AGP) között.
    * Meghatározza a gép sebességét és a támogatott RAM típusát/mennyiségét.
2.  **Southbridge (Déli híd):**
    * A "lassabb" I/O eszközökért felel.
    * Ide csatlakoznak: **HDD/SSD (SATA)**, **USB portok**, **Hálózati kártya**, **Hangkártya**, **BIOS/UEFI**.

---

## 3. Memóriák és Tárolók

### RAM (Random Access Memory) típusok
Az adatok ideiglenes tárolására szolgál. Kikapcsoláskor törlődik (volatilis).
* **DRAM (Dinamikus RAM):** Folyamatos elektromos frissítést igényel. Ez a "hagyományos" rendszermemória.
* **SRAM (Statikus RAM):** Nagyon gyors, drága, nem kell frissíteni. **Cache (gyorsítótár)** memóriaként használják a CPU-ban (L1, L2, L3 cache).
* **ECC (Error Correction Code):** Szervereknél használt RAM. Képes detektálni és javítani az egybites hibákat (stabilitás).
* **SODIMM:** Kisebb méretű modulok, **laptopokhoz** és nyomtatókhoz.

### ROM (Read-Only Memory)
* **Funkció:** Olyan utasításokat tárol, amikre a gépnek induláskor szüksége van (BIOS/UEFI firmware).
* **EEPROM / Flash:** Elektromosan törölhető és újraírható típus (ez teszi lehetővé a BIOS frissítést).

### Háttértárak
* **HDD (Merevlemez):** Mágneses adattárolás, forgó tányérok. Jellemzője az **RPM** (fordulat/perc). Érzékeny a rázkódásra.
* **SSD (Solid State Drive):** Félvezetős (Flash) technológia. Nincs mozgó alkatrész.
    * *Előny:* Gyorsabb, halkabb, kevesebbet fogyaszt, **ellenáll a vibrációnak** (ipari környezetbe ez kell!).
* **Optikai meghajtók:** CD/DVD/Blu-ray (lézeres leolvasás).

---

## 4. Portok és Speciális Eszközök

| Eszköz / Port | Típus | Gyakorlati haszon / Magyarázat |
| :--- | :--- | :--- |
| **KVM Switch** | Periféria | **K**eyboard, **V**ideo, **M**ouse. Több PC vezérlése egyetlen billentyűzet/egér/monitor szettel. (Szervertermekben alapvető). |
| **Biometrikus szkenner** | Bemenet | Azonosítás (ujjlenyomat, retina) – fizikai biztonsági beléptetéshez. |
| **Digitizer** | Bemenet | Grafikus tábla, tollal való precíz rajzoláshoz/aláíráshoz. |
| **NFC** | Vezeték nélküli | "Tap and pay" (érintéses fizetés), beléptetőkártyák. |
| **HDMI / DisplayPort** | A/V Port | Digitális videó és audió átvitel egyetlen kábelen (pl. PC -> TV). |
| **24-pin ATX** | Tápcsatlakozó | Ez látja el árammal magát az alaplapot. |

---

## 5. Összefüggések (Logikai lánc)

1.  **Hűtés:** Két típusa van.
    * *Aktív:* Ventilátorral mozgatja a levegőt (kell neki áram).
    * *Passzív:* Csak hűtőborda (borda felületnövelés), csendes, de kevésbé hatékony nagy hőnél.
2.  **Kompatibilitás:** Memóriabővítésnél nem elég, hogy "DDR3". A chipsettől függ, hogy támogatja-e az adott sebességet (MHz) és méretet.
3.  **Tárhelybővítés:** Ha nincs elég SATA port, bővítőkártyával (pl. RAID kártya) lehet növelni a csatlakoztatható lemezek számát.

---

## 6. OSZTV Ellenőrző Kérdések (A Question Bank alapján)

**1. Melyik chipset komponens felel a CPU és a RAM közötti kommunikációért?**
* *Válasz: Northbridge (Északi híd).*

**2. Milyen típusú memóriát használnak a CPU gyorsítótárához (Cache), és miért?**
* *Válasz: SRAM-ot, mert ez a leggyorsabb technológia (gyorsabb, mint a DRAM) és nem igényel frissítést.*

**3. Egy rázkódásnak kitett ipari környezetbe (pl. gyárcsarnok) milyen háttértárat ajánlana?**
* *Válasz: SSD-t, mert nincs benne mozgó alkatrész, így nem érzékeny a vibrációra.*

**4. Mire szolgál az ECC funkció a memóriák esetében?**
* *Válasz: Adathibák (bit hibák) felismerésére és javítására (kritikus fontosságú szervereknél).*

**5. Egy technikus két külön számítógépet (pl. egy szervert és egy munkaállomást) szeretne kezelni egyetlen monitorral és billentyűzettel. Milyen eszközt használjon?**
* *Válasz: KVM kapcsolót (switch).*

**6. Milyen feszültséget használ a számítógép a lemezmeghajtók motorjainak működtetéséhez?**
* *Válasz: +12 Volt.*

**7. Melyik kártya teszi lehetővé, hogy analóg videofelvevőről (VHS) digitalizáljunk anyagot a számítógépre?**
* *Válasz: Video capture card (Videó digitalizáló kártya).*

# 3. Fejezet: Haladó hardverismeretek (ITE)

Ez a szakasz a PC működésének mélyebb összefüggéseit tárgyalja. OSZTV-n kiemelten figyelj a **RAID szintekre**, a **speciális munkaállomásokra** és a **BIOS

# 3. Fejezet: Haladó hardverismeretek (ITE)

Ez a fejezet a rendszer indulásától a speciális konfigurációkig mindent lefed. Az OSZTV tesztekben a RAID szintek és a feszültségértékek állandó kérdések.

---

## 1. A számítógép indulása (Boot folyamat)

### POST, BIOS és UEFI
* **POST (Power-On Self-Test):** A gép bekapcsolásakor lefutó önteszt. Ha hibát talál (pl. nincs RAM), **sípoló kódokkal** (beep codes) jelzi.
* **BIOS (Basic Input/Output System):** Régi szabvány. Alapvető hardverbeállításokat tárol. Csak billentyűzettel vezérelhető, max 2.2 TB-os partíciókat kezel.
* **UEFI (Unified Extensible Firmware Interface):** Modern szabvány. Támogatja az egeret, grafikus felülete van, és kezeli a 2.2 TB feletti meghajtókat is (**GPT** partíciós tábla).
* **CMOS:** Egy kis memória chip az alaplapon, ami a BIOS beállításokat tárolja. Egy **gombelem** (CR2032) táplálja; ha ez lemerül, a gép minden reggel elfelejti az órát és a beállításokat.

### BIOS/UEFI Biztonsági funkciók
* **Password:** Felhasználói vagy rendszergazdai jelszó a belépéshez.
* **LoJack:** Ellopott laptopok távoli nyomon követése és zárolása (hardver szinten).
* **Secure Boot:** Csak digitálisan aláírt, megbízható operációs rendszert enged betölteni (védelem a rootkitek ellen).
* **Drive Encryption (TPM):** Hardveres titkosítás a háttértárhoz.

---

## 2. Elektromosság és Védelem

### Alapfogalmak
* **Feszültség (Voltage - V):** Az elektronokat mozgató "nyomás".
* **Áramerősség (Current - A):** A körben másodpercenként átfolyó elektronok száma.
* **Ellenállás (Resistance - Ohm):** Az áramfolyást gátló tényező.
* **Teljesítmény (Power - Watt):** $P = V \cdot I$ (Feszültség $\times$ Áramerősség). Ez határozza meg, mekkora tápegység kell.

### Tápellátási zavarok
* **Brownout:** Tartós feszültségesés (nem szűnik meg az áram, de gyenge).
* **Blackout:** Teljes áramkimaradás.
* **Spike / Surge:** Hirtelen túlfeszültség (pl. villámcsapás).
* **Védelem:** Az **Inline UPS** (szünetmentes táp) akkumulátora állandó szinten tartja a feszültséget, így véd a brownout és blackout ellen is.

---

## 3. RAID (Redundant Array of Independent Disks)

A RAID célja a **teljesítmény növelése** vagy az **adatbiztonság (hibatűrés)**.



| RAID Szint | Technológia | Min. lemez | Leírás |
| :--- | :--- | :---: | :--- |
| **RAID 0** | **Striping** (Csíkozás) | 2 | Maximális sebesség, **nulla hibatűrés**. Ha egy lemez meghal, minden adat vész. |
| **RAID 1** | **Mirroring** (Tükrözés) | 2 | Hibatűrés: az adatok mindkét lemezen megvannak. Lassabb írás. |
| **RAID 5** | **Striping + Parity** | 3 | Egyensúly. 1 lemez kiesését bírja ki. Jó sebesség és biztonság. |
| **RAID 6** | **Double Parity** | 4 | 2 lemez egyidejű kiesését is kibírja. |
| **RAID 10** | **1 + 0 kombináció** | 4 | Tükrözött csíkok. Gyors és nagyon biztonságos. |

---

## 4. Speciális Munkaállomások (OSZTV kedvenc!)

Melyik gépbe mi a legfontosabb alkatrész?

* **Virtualizációs gép:** Maximális **RAM** mennyiség és sok **CPU mag** (minden VM-nek saját erőforrás kell).
* **Grafikus/CAD tervező (CAx):** Erős **GPU** (videókártya) és sok RAM.
* **Audio/Video vágó:** Gyors háttértár (SSD), két monitor, minőségi hangkártya.
* **Vékony kliens (Thin Client):** Gyenge hardver, szinte minden a szerveren fut. Kell egy stabil **hálózati kapcsolat**.
* **NAS (Network Attached Storage):** Sok nagy kapacitású HDD, gigabites hálózat, RAID támogatás.

---

## 5. OSZTV Ellenőrző Kérdések (A beküldött bank alapján)

**1. Melyik két alkatrészt kell általában egyszerre cserélni az alaplappal?**
* *Válasz: CPU és RAM.*

**2. Mi a különbség a Brownout és a Sag között?**
* *Válasz: A Brownout tartós feszültségesés, a Sag csak rövid ideig tart.*

**3. Melyik RAID szint biztosít hibatűrést 2 meghajtó kiesése esetén is?**
* *Válasz: RAID 6.*

**4. Mi a LoJack funkciója?**
* *Válasz: Lehetővé teszi az ellopott eszköz távoli helymeghatározását, zárolását vagy az adatok törlését.*

**5. Mi történik, ha a CMOS elem lemerül?**
* *Válasz: A gép minden kikapcsolás után elveszíti a BIOS beállításokat és a pontos időt.*

**6. Milyen egységben mérjük a másodpercenként átfolyó elektronok számát?**
* *Válasz: Amper (Áramerősség).*

**7. Milyen technológiát használunk, ha egy egérrel és billentyűzettel akarunk 3 gépet vezérelni?**
* *Válasz: KVM switch.*

# 4. Fejezet: Megelőző karbantartás és hibaelhárítás (ITE)

Ez a fejezet a gyakorlati verseny egyik legfontosabb része. A hibaelhárítási folyamat lépéseit és a hardveres hibajelenségeket magabiztosan kell ismerned az OSZTV-hez.

---

## 4.1 Megelőző karbantartás
A rendszeres karbantartás célja a meghibásodások megelőzése, az alkatrészek élettartamának növelése és az adatvédelem javítása.

### Hardveres karbantartási feladatok
* **Poreltávolítás:** A por hőszigetelő, ami túlmelegedést okoz. Tisztítás sűrített levegővel.
    * **Fontos:** A ventilátorokat le kell fogni tisztítás közben, mert a túlpörgetés tönkreteheti a csapágyat vagy áramot indukálhat vissza az alaplapba.
* **Belső alkatrészek:** Meglazult kábelek, RAM modulok ellenőrzése ("reseating").
* **Környezet:** Védelem a szélsőséges hőmérséklet, magas páratartalom és elektromos interferencia (EMI) ellen.

### Szoftveres karbantartási feladatok
* **Biztonság:** Antivírus definíciók és operációs rendszer frissítése (Patching).
* **Lemezkezelés:** Fájlrendszer ellenőrzése (**chkdsk**), töredezettségmentesítés (csak HDD-n, SSD-n tilos/felesleges).

---

## 4.2 A hibaelhárítás 6 lépése (OSZTV sorrend!)

Ezt a pontos sorrendet a tesztekben szinte minden évben kérik:

1.  **A probléma azonosítása:** Kérdezzük ki a felhasználót (nyitott és zárt kérdések), nézzük meg a hibaüzeneteket és az **Eseménynaplót (Event Viewer)**.
2.  **A lehetséges okok elméletének felállítása:** Listázzuk a lehetőségeket, kezdve a legegyszerűbbel (pl. nincs bedugva a tápkábel).
3.  **Az elmélet tesztelése:** Próbáljuk ki a feltételezett megoldást. Ha nem működik, új elméletet kell felállítani.
4.  **Megoldási terv és végrehajtás:** Ha megvan a biztos ok, tervezzük meg a javítást és hajtsuk végre.
5.  **Rendszerellenőrzés:** Győződjünk meg róla, hogy minden funkció helyreállt (pl. nyomtassunk tesztoldalt), és vezessünk be megelőző intézkedéseket.
6.  **Dokumentálás:** Jegyezzünk fel mindent: a hiba leírását, a próbálkozásokat és a végső megoldást.

---

## 4.3 Gyakori problémák és megoldások (Bank alapján)

| Jelenség | Valószínű ok | Megoldás |
| :--- | :--- | :--- |
| **A gép 5 perc után lefagy/leáll** | CPU túlmelegedés | Hűtő tisztítása, pasztázás ellenőrzése. |
| **A HDD LED folyamatosan villog** | Kevés a RAM | Memóriabővítés (a gép virtuális memóriát használ, ami lassú). |
| **Égett elektronika szaga** | Tápegység hiba | Tápegység azonnali cseréje. |
| **Elvész a dátum és az idő** | CMOS elem lemerült | CR2032 gombelem cseréje. |
| **Nincs kép az új videókártyával** | Integrált videó / Táp | Letiltás BIOS-ban vagy PCIe tápkábel csatlakoztatása. |
| **Induláskor sípol a gép** | RAM/VGA érintkezési hiba | Alkatrész kivétele és határozott visszahelyezése. |

---

## 4.4 OSZTV Fontos megjegyzések

* **Adatbiztonság:** Bármilyen javítás előtt az első lépés a **biztonsági mentés (Backup)**. Ha ez nem lehetséges, írassunk alá felelősségvállalási nyilatkozatot (liability release).
* **Eszközkezelő (Device Manager):** Itt látszanak a hardveres konfliktusok. Sárga felkiáltójel = driver hiba.
* **Escalation (Eszkaláció):** Ha a hiba meghaladja a tudásunkat, dokumentáljuk az eddigieket és adjuk át a szint feletti technikusnak.
* **BIOS:** Ha a BIOS minden indításnál újra felismeri a hardvereket, az szintén a **CMOS elem** hibája.

---

# 5. Fejezet: Hálózatok (ITE)

Ez a fejezet a hálózati eszközök, protokollok és kábelezési technológiák világába vezet be. Az OSZTV-n kiemelt téma a portszámok ismerete és a hálózati rétegek felépítése.

---

## 5.1 Hálózati típusok és topológiák

### Hálózattípusok kiterjedés szerint
* **PAN (Personal Area Network):** Személyes hálózat (pl. Bluetooth eszközök kapcsolata).
* **LAN (Local Area Network):** Helyi hálózat (lakás, iroda).
* **WAN (Wide Area Network):** Távoli hálózatokat köt össze, általában szolgáltató biztosítja (pl. Internet).
* **WLAN:** Vezeték nélküli LAN (Wi-Fi).

### Topológiák
* **Fizikai topológia:** Az eszközök és kábelek fizikai elrendezése.
* **Logikai topológia:** Az adatok áramlásának útja a hálózaton.
    * *Csillag topológia:* Minden eszköz egy központi kapcsolóhoz (switch) csatlakozik.

---

## 5.2 Protokollok és Portszámok (OSZTV KRITIKUS!)

### Szállítási réteg (Transport Layer)
* **TCP (Transmission Control Protocol):** Megbízható, kapcsolat-orientált. Nyugtázza az adatokat (pl. HTTP, Email).
* **UDP (User Datagram Protocol):** Gyors, "best-effort" jellegű. Nincs nyugtázás, jó streameléshez és DNS-hez.

### Legfontosabb portszámok (Ezeket tudni kell!)
| Port | Protokoll | Leírás |
| :--- | :--- | :--- |
| **20/21** | **FTP** | Fájlátvitel (Adat/Vezérlés) |
| **22** | **SSH** | Biztonságos távoli elérés |
| **23** | **Telnet** | Nem biztonságos távoli elérés |
| **25** | **SMTP** | Levélküldés |
| **53** | **DNS** | Névfeloldás (Név -> IP cím) |
| **67/68** | **DHCP** | Automatikus IP cím kiosztás (Szerver/Kliens) |
| **80** | **HTTP** | Webes forgalom (nem titkosított) |
| **110** | **POP3** | Levélletöltés |
| **143** | **IMAP** | Levélkezelés a szerveren |
| **443** | **HTTPS** | Titkosított webes forgalom |
| **3389** | **RDP** | Távoli asztali kapcsolat |

---

## 5.3 Hálózati eszközök

* **Switch (Kapcsoló):** **MAC címek** alapján továbbítja az adatkereteket a megfelelő portra.
* **Router (Forgalomirányító):** **IP címek** alapján irányítja a forgalmat különböző hálózatok között.
* **Access Point (AP):** Vezeték nélküli kapcsolódást tesz lehetővé.
* **Modem:** Átalakítja a digitális jelet a szolgáltató közegének megfelelő formátumra (pl. DSL vagy kábel).
* **Firewall (Tűzfal):** Szűri a forgalmat a biztonsági szabályok alapján.
* **PoE (Power over Ethernet):** Az ethernet kábelen keresztül áramot is küld (pl. IP telefonoknak vagy AP-knak).



---

## 5.4 Hálózati kábelezés

### Rézkábelek (Twisted Pair)
* **UTP:** Árnyékolatlan csavart érpár (leggyakoribb).
* **STP:** Árnyékolt csavart érpár (véd az elektromos interferencia - EMI - ellen).
* **Bekötési szabványok (T568A vs T568B):** A különbség a **zöld** és a **narancs** párok felcserélésében van.
    * *Egyenes bekötés:* Mindkét vég azonos (A-A vagy B-B).
    * *Cross-over (Kereszt):* Egyik vége A, másik B (azonos típusú eszközök összekötésére, pl. PC-PC).

### Optikai kábel (Fiber Optic)
* Fényjelekkel továbbít, nem érzékeny az elektromos zavarokra (EMI/RFI).
* **Single-mode:** Nagy távolságokra, lézerrel.
* **Multi-mode:** Rvidebb távolságokra, LED-del.

---

## 5.5 OSZTV Ellenőrző kérdések (Bank alapján)

**1. Melyik eszköz hoz döntést a cél MAC-cím alapján?**
* *Válasz: Switch (Kapcsoló).*

**2. Hány eszközt tud egy Bluetooth eszköz egyszerre kiszolgálni?**
* *Válasz: 7.*

**3. Melyik portot használja a DHCP kliens?**
* *Válasz: UDP 68 (a szerver a 67-est).*

**4. Mi a különbség a T568A és T568B szabvány között?**
* *Válasz: A zöld és a narancssárga érpárak fel vannak cserélve.*

**5. Melyik vezeték nélküli okosotthon szabvány támogat maximum 232 eszközt?**
* *Válasz: Z-Wave.*

**6. Mi a Syslog szerver feladata?**
* *Válasz: A hálózati eszközökről érkező naplóüzenetek központi tárolása.*

**7. Melyik típusú internetkapcsolat a leggyorsabb általában?**
* *Válasz: Optikai (Fiber).*

---

# 6. Fejezet: Hálózatok a gyakorlatban (ITE)

Ez a fejezet a hálózati elmélet gyakorlati alkalmazására fókuszál. Az OSZTV-n kiemelt téma az IP-címek formátuma, a DHCP működése és a hibaelhárítási parancsok.

---

## 6.1 Hálózati címzés (MAC és IP)

### MAC-cím (Fizikai cím)
* **Hossz:** 48 bit (12 hexadecimális karakter).
* **Felépítés:** Az első 24 bit az **OUI** (gyártó azonosítója), az utolsó 24 bit az egyedi eszközazonosító.
* **Réteg:** Adatkapcsolati réteg (Layer 2).

### IPv4 címzés
* **Hossz:** 32 bit (4 oktett, pontokkal elválasztva).
* **Alhálózati maszk:** Meghatározza, melyik rész a hálózat és melyik a host (pl. 255.255.255.0 = /24).
* **Privát címtartományok:** 10.x.x.x, 172.16.x.x - 172.31.x.x, 192.168.x.x.
* **APIPA (Link-local):** Ha nincs DHCP szerver, a Windows a **169.254.x.x** tartományból oszt ki címet. Ezzel csak a helyi hálózaton lehet kommunikálni, internet nincs!

### IPv6 címzés
* **Hossz:** 128 bit (8 hextet, kettőspontokkal elválasztva).
* **Tömörítés:** 1. Vezető nullák elhagyhatók.
    2. Egymást követő csupa nulla blokkok egy darab `::`-al helyettesíthetők (de csak egyszer!).
* **Példa:** `2001:0db8:0000:0000:0000:0000:0000:0001` -> `2001:db8::1`.



---

## 6.2 Hálózati szolgáltatások és beállítások

* **DHCP (Dynamic Host Configuration Protocol):** Automatikusan oszt ki IP-címet, maszkot, átjárót és DNS-t.
* **DNS (Domain Name System):** Hostneveket (pl. google.com) fordít IP-címekre. Ha az IP-t tudod pingelni, de a nevet nem, a DNS a hibás.
* **Default Gateway (Alapértelmezett átjáró):** Ez a router belső lába. Ezen keresztül hagyja el az adat a helyi hálózatot az Internet felé.
* **ICMP:** Ezt használja a **ping** és a **tracert** hibaüzenetek küldésére.

---

## 6.3 Hálózati biztonság és Tűzfal

* **Port Forwarding (Porttovábbítás):** Lehetővé teszi, hogy külső internetes forgalom elérjen egy belső hálózati eszközt (pl. webszervert).
* **Port Triggering:** Dinamikus portnyitás: egy kimenő forgalom "kioldja" a hozzátartozó bejövő portot.
* **DMZ (Demilitarizált zóna):** Egy belső gépet teljesen kitesz a külső hálózatnak (minden portot ráirányít).
* **MAC szűrés:** Csak meghatározott fizikai című eszközöknek enged csatlakozást.

---

## 6.4 Hibaelhárítási parancsok (CLI)

| Parancs | Funkció |
| :--- | :--- |
| **ipconfig /all** | Minden hálózati adat (IP, MAC, DHCP, DNS) kilistázása. |
| **ipconfig /release** | Aktuális DHCP-s IP cím elengedése. |
| **ipconfig /renew** | Új IP cím kérése a DHCP szervertől. |
| **ping 127.0.0.1** | Loopback teszt: a saját hálózati kártyánk (stack) működik-e. |
| **tracert [cím]** | Megmutatja az utat (routereket) a célig. Segít megtalálni, hol szakad meg a kapcsolat. |
| **nslookup [név]** | DNS szerver tesztelése (névfeloldás). |

---

## 6.5 OSZTV Ellenőrző kérdések (Bank alapján)

**1. Mit jelent, ha a gép 169.254.x.x IP-címet kapott?**
* *Válasz: A gép nem érte el a DHCP szervert (APIPA cím).*

**2. Mi a különbség a port forwarding és a port triggering között?**
* *Válasz: A forwarding állandó szabály, a triggering csak akkor nyit portot, ha egy belső alkalmazás kéri.*

**3. Miért érdemes a Wi-Fi csatornát 1-es, 6-os vagy 11-es értékre állítani?**
* *Válasz: Mert ezek nem fedik át egymást, így elkerülhető az interferencia.*

**4. Hogyan írható le tömören a következő IPv6 cím: 2001:0db8:0000:000a:0000:0000:0000:0001?**
* *Válasz: 2001:db8:0:a::1.*

**5. Mi a teendő, ha tudod pingelni a routert, de az internetet nem éred el?**
* *Válasz: Ellenőrizni kell az alapértelmezett átjárót (Default Gateway) és a DNS beállításokat.*

**6. Milyen szerszám kell egy RJ-45-ös csatlakozó felszereléséhez?**
* *Válasz: Krimpelő fogó (Crimping tool).*

---

# 7. Fejezet: Laptopok és egyéb mobileszközök (ITE)

A mobileszközöknél a legfőbb szempont a mobilitás, a súly és az energiafogyasztás. Az OSZTV-n gyakran kérdezik a laptop-specifikus alkatrészeket és az energiagazdálkodási állapotokat.

---

## 1. Laptop hardver és sajátosságok

A laptopok alkatrészei gyakran egyediek (proprietary), nem cserélhetőek szabadon más márkák között.

* **Alaplap:** Nem szabványos (nem ATX). A forma és a csatlakozók elhelyezkedése gyártó- és modellfüggő.
* **CPU:** Kevesebb hőt termel és kevesebb energiát fogyaszt.
    * **CPU Throttling:** Dinamikusan változtatja az órajelet (lassítja a processzort), hogy hűtse az eszközt és kímélje az akkumulátort.
* **RAM:** A laptopok **SODIMM** (Small Outline DIMM) modulokat használnak. 
* **Kijelző:** A keretben (lid) futnak a Wi-Fi antennák, ott van a webkamera és a mikrofon is.
* **Bővítés:**
    * **Mini-PCIe / M.2:** Belső kártyákhoz (pl. Wi-Fi kártya, SSD).
    * **ExpressCard:** Régebbi külső bővítőkártya hely.

### Kijelző technológiák
* **Digitizer (Digitalizáló):** Az érintőképernyő azon rétege (üveglapja), amely az érintést (analóg) digitális jellé alakítja.
* **Inverter:** Csak a régebbi, **CCFL** háttérvilágítású LCD kijelzőknél található meg. Feladata: az egyenáramot (DC) váltóárammá (AC) alakítani a fénycsőhöz.
    * *Megjegyzés:* A modern LED-es kijelzőknek **NINCS** invertere!
* **Cutoff Switch (Lezáró kapcsoló):** Érzékeli, ha lecsukod a fedelet, és kikapcsolja a képernyőt (vagy alvó módba teszi a gépet). Ha beragad, a gép bekapcsol, de a kép sötét marad.

---

## 2. Energiagazdálkodás (ACPI szabvány)

Az **ACPI** (Advanced Configuration and Power Interface) kezeli az energiaszinteket a BIOS és az OS között.

| Állapot | Leírás |
| :--- | :--- |
| **S0** | A számítógép be van kapcsolva, a CPU dolgozik. |
| **S3 (Sleep)** | **Alvó állapot (Standby).** A CPU kikapcsol, de a RAM kap áramot. Gyors ébredés, de ha lemerül az akku, az adat elvész. |
| **S4 (Hibernate)** | **Hibernálás.** A RAM tartalma a merevlemezre íródik. A gép teljesen áramtalanítva van. Lassabb ébredés, de adatbiztos. |

---

## 3. Mobil kommunikáció és Protokollok

### Vezeték nélküli kapcsolatok
* **Bluetooth:** Személyes hálózat (PAN). Egyszerre max. **7 eszközt** kezel. A kapcsolat létrehozásához **párosítás (pairing)** szükséges.
* **NFC (Near Field Communication):** Nagyon rövid hatótávolság. Fizetéshez, nyomtatáshoz vagy gyors adatcseréhez.
* **Tethering / Hotspot:** A mobiltelefon internetkapcsolatának megosztása más eszközökkel (Wi-Fi-n, Bluetooth-on vagy USB-n keresztül).
* **Repülőgép mód:** Kikapcsol minden rádiójelet (Wi-Fi, GSM, BT, GPS).

### E-mail protokollok mobilon
* **POP3:** Letölti a levelet a szerverről a telefonra (és törölheti a szerverről).
* **IMAP:** Szinkronizálja a leveleket (a szerveren marad az eredeti).
* **SMTP:** Levélküldésre szolgál.
* **MIME:** Ez a szabvány teszi lehetővé, hogy **képeket és dokumentumokat csatoljunk** az e-mailekhez (az eredeti e-mail csak szöveget tudott).

### Egyéb fogalmak
* **IMSI:** A SIM kártyán lévő egyedi előfizetői azonosító.
* **IMEI:** A telefonkészülék egyedi hardverazonosítója.

---

## 4. Karbantartás és Alkatrészcsere

### Tisztítás
* **Megfelelő szerek:** Enyhe tisztítószer (**mild cleaning solution**), desztillált víz, szöszmentes törlőkendő, fültisztító pálcika.
* **TILOS:** Ammóniát vagy alkoholt tartalmazó szert fújni a kijelzőre (károsítja a bevonatot).

### Csere kategóriák
* **CRU (Customer Replaceable Unit):** A felhasználó által cserélhető (pl. akku, ha kivehető, vagy régebben a HDD/RAM szerelőablakon át).
* **FRU (Field Replaceable Unit):** Szerviztechnikus által cserélhető (pl. alaplap, kijelző, forrasztott alkatrészek).

---

## 5. OSZTV Ellenőrző kérdések (Bank alapján)

**1. Melyik alkatrészre van szükség a régebbi CCFL LCD kijelzők háttérvilágításához?**
* *Válasz: Inverter.*

**2. Milyen típusú memóriamodult használnak a laptopok?**
* *Válasz: SODIMM.*

**3. Melyik technológia teszi lehetővé képek és dokumentumok csatolását e-mailekhez?**
* *Válasz: MIME.*

**4. Mi a teendő, ha a laptop bekapcsol (LED világít), de a kijelző sötét?**
* *Válasz: Ellenőrizni kell a lezáró kapcsolót (cutoff switch), hogy nem ragadt-e be.*

**5. Melyik ACPI állapot jelenti a hibernálást?**
* *Válasz: S4.*

**6. Milyen tisztítószert ajánlott használni a laptopokhoz?**
* *Válasz: Enyhe tisztítószert (mild cleaning solution) és szöszmentes kendőt.*

---

# 8. Fejezet: Nyomtatók (ITE)

Ez a fejezet a nyomtatási technológiákat, azok karbantartását és hibaelhárítását tárgyalja. Kiemelt fontosságú a lézernyomtatás 7 lépése!

---

## 1. Nyomtatótípusok és működésük

### A. Tintasugaras (Inkjet)
* **Működés:** Folyékony tintát lő a papírra fúvókákon keresztül.
* **Típusai:**
    * *Termikus (Thermal):* Hővel buborékot képez, ami kilövi a tintát (HP, Canon).
    * *Piezoelektromos:* Kristály rezgése lövi ki a tintát (Epson).
* **Előny:** Olcsó beszerzés, kiváló fotóminőség.
* **Hátrány:** Drága patron, a fúvóka beszáradhat, a nyomat elmosódhat, ha nedvesség éri.

### B. Lézernyomtató (Laser)
* **Működés:** Tonert (porfestéket) éget a papírra lézersugár és egy fényérzékeny henger segítségével.
* **Előny:** Gyors, alacsony lapköltség, tartós nyomat.
* **Hátrány:** Drága induló ár, magas energiafogyasztás (főleg a beégető miatt).

### C. Hőnyomtató (Thermal)
* **Működés:** Speciális hőérzékeny papírt (blokkpapír) feketít meg a fűtőelem.
* **Használat:** Blokkok, címkék nyomtatása (pl. bolti kassza).
* **Karbantartás:** A fűtőelemet **izopropil-alkohollal** kell tisztítani.

### D. Mechanikus / Ütő (Impact / Dot Matrix)
* **Működés:** Tűkkel üt rá egy festékszalagra (mint az írógép).
* **Használat:** Indigós (többpéldányos) számlák nyomtatása. Ez az egyetlen típus, ami képes egyszerre több példányt (karbonpapírral) nyomtatni.

### E. 3D nyomtatók
* **Működés:** Műanyagszálat (filament) olvaszt meg és rétegenként építi fel a tárgyat (**additív gyártás**).

---

## 2. A lézernyomtatás 7 lépése (OSZTV KRITIKUS!)

Ezt a sorrendet **kötelező** tudni, gyakori sorbarendezős feladat:

1.  **Processing (Feldolgozás):** A nyomtató a memóriájába fogadja az adatot és bittérképpé alakítja.
2.  **Charging (Töltés):** A fényhenger (dob/drum) felületét negatív töltéssel látja el a töltőhenger (PCR).
3.  **Exposing (Levilágítás):** A lézer "rárajzolja" a képet a hengerre. Ahol a lézer éri, ott a henger elveszíti a töltését (semleges lesz).
4.  **Developing (Előhívás):** A toner (negatív töltésű festékpor) rátapad a henger azon részeire, amit a lézer ért (a semleges helyekre).
5.  **Transferring (Átvitel):** A papír pozitív töltést kap, így magához vonzza a tonert a hengerről.
6.  **Fusing (Beégetés):** A fűtőhenger (Fuser) hővel és nyomással tartósan ráégeti a festéket a papírra.
7.  **Cleaning (Tisztítás):** A maradék festékport letörli a hengerről egy gumilapát, a töltést pedig semlegesítik.

---

## 3. Csatlakozás és Megosztás

* **Csatlakozók:**
    * Modern: USB, Ethernet (RJ-45), Wi-Fi (802.11), Bluetooth.
    * Régi (Legacy): Párhuzamos port (LPT), Soros port.
* **Nyomtató nyelvek:**
    * **PDL (Page Description Language):** A számítógép ezen a nyelven beszél a nyomtatóval. Fő típusok: **PCL** (HP szabvány) és **PostScript** (Adobe szabvány, grafikai munkákhoz).
* **Megosztás:**
    * *Print Server:* Lehet egy külön eszköz, vagy egy szoftveres szolgáltatás egy PC-n. Kezeli a nyomtatási sorokat (Queue) és a jogosultságokat.
* **Virtuális nyomtatás:** Nem papírra, hanem fájlba nyomtatunk (pl. Print to PDF, Print to XPS).

---

## 4. Karbantartás és Hibaelhárítás

| Jelenség | Valószínű ok | Megoldás |
| :--- | :--- | :--- |
| **Papírelakadás (Jam)** | Párás papír, rossz görgők, rossz papírtípus | Papír cseréje, görgők tisztítása (sűrített levegő, víz) vagy cseréje. |
| **Szellemképes nyomat** | Hiba a tisztítási fázisban vagy a dobban | Dobegység (Drum) vagy lehúzópenge cseréje. |
| **Függőleges csíkok** | Sérült dob vagy koszos koronahuzal | Dob cseréje. |
| **A festék lejön a papírról** | A Fuser (beégető) nem működik | Fuser egység cseréje. |
| **Halvány nyomat** | Kevés a toner vagy festéktakarékos mód | Toner felrázása vagy cseréje; beállítások ellenőrzése. |
| **"Access Denied"** | Jogosultsági hiba | Ellenőrizni a megosztási beállításokat a Security fülön. |

---

## 5. OSZTV Ellenőrző kérdések (Bank alapján)

**1. Melyik folyamat során tapad a toner a fényhengerre a lézernyomtatásban?**
* *Válasz: Developing (Előhívás).*

**2. Hogyan hosszabbítható meg a hőnyomtató élettartama?**
* *Válasz: A fűtőelem rendszeres tisztításával izopropil-alkohollal.*

**3. Mi a teendő, ha a lézernyomtató nyomata elmosódik, és a festék kézzel letörölhető a papírról?**
* *Válasz: Ki kell cserélni a beégető egységet (Fuser), mert nem melegít eléggé.*

**4. Melyik nyomtatótípus használ festékszalagot és tűket, és alkalmas indigós számlák nyomtatására?**
* *Válasz: Mechanikus / Ütő nyomtató (Impact / Dot Matrix).*

**5. Mi a Print Server fő feladata?**
* *Válasz: Kezeli a nyomtatási sorokat (Queue) és biztosítja a nyomtató elérését több felhasználó számára.*

**6. Melyik szoftveres komponens teszi lehetővé, hogy az operációs rendszer kommunikáljon a nyomtatóval?**
* *Válasz: Nyomtató illesztőprogram (Printer Driver).*

**7. Melyik vezeték nélküli módban kommunikálhat két eszköz közvetlenül egymással (pl. laptop és nyomtató) router nélkül?**
* *Válasz: Ad hoc mód.*

---

# 9. Fejezet: Virtualizáció és felhőtechnológiák (ITE)

Ez a fejezet a modern IT alapjait képező virtualizációt és a felhőszolgáltatási modelleket tárgyalja. Az OSZTV-n gyakori kérdés a felhőmodellek (SaaS, PaaS, IaaS) megkülönböztetése.

---

## 9.1 Virtualizáció alapjai

### Mi a virtualizáció?
Egy fizikai számítógépen (Host) szoftveres úton több virtuális gépet (Guest) futtatunk egyszerre.
* **Host (Gazdagép):** A fizikai hardver.
* **Guest (Vendég):** A virtuális gép (VM).
* **Hypervisor (VMM - Virtual Machine Monitor):** A szoftver, ami kezeli a virtuális gépeket és elosztja az erőforrásokat (CPU, RAM).

### A virtualizáció előnyei
* **Költségcsökkentés:** Kevesebb fizikai szerver kell (jobb hardverkihasználás).
* **Energiahatékonyság:** Kevesebb áramfogyasztás és hűtés.
* **Gyorsaság:** Új szerverek percek alatt létrehozhatók.
* **Biztonság:** A virtuális gépek elszigeteltek (Sandbox); ha egy VM vírusos lesz, a többi biztonságban marad.

### Hypervisor típusok (OSZTV KRITIKUS!)
1.  **Type 1 (Bare-metal / Natív):**
    * Közvetlenül a hardverre települ (nincs alatta Windows/Linux).
    * Gyorsabb és biztonságosabb. Szervereknél ezt használják.
    * *Példák:* VMware ESXi, Microsoft Hyper-V, Xen.
2.  **Type 2 (Hosted / Gazdagép-alapú):**
    * Egy futó operációs rendszerre (pl. Windows 10) települ, mint egy program.
    * Otthoni felhasználásra, tesztelésre.
    * *Példák:* VMware Workstation, Oracle VirtualBox.

### Kliens-oldali virtualizáció (VDI)
* **VDI (Virtual Desktop Infrastructure):** A felhasználó asztala (Desktop) nem a saját gépén fut, hanem egy központi szerveren. A felhasználó csak a képet kapja meg a "vékony kliensére".
    * *Perzisztens:* A felhasználó beállításai megmaradnak.
    * *Nem perzisztens:* Kilépéskor minden visszaáll alapállapotba (pl. iskolai labor).

---

## 9.2 Felhőszolgáltatások (Cloud Computing)

A "felhő" valójában mások szervereinek bérlése az interneten keresztül.

### Szolgáltatási modellek (Service Models) - "XaaS"
1.  **SaaS (Software as a Service):**
    * Szoftver szolgáltatásként. A felhasználó csak használja a kész programot böngészőből. Nem kell telepíteni semmit.
    * *Példa:* Gmail, Office 365, Dropbox.
2.  **PaaS (Platform as a Service):**
    * Platform szolgáltatásként. Fejlesztőknek ad környezetet (OS + fejlesztőeszközök + adatbázis), hogy saját alkalmazást írjanak.
    * *Példa:* Google App Engine, Microsoft Azure (fejlesztői része).
3.  **IaaS (Infrastructure as a Service):**
    * Infrastruktúra szolgáltatásként. A felhasználó "vasat" (virtuális CPU-t, RAM-ot, tárhelyet) bérel, és ő telepíti rá az operációs rendszert.
    * *Példa:* Amazon AWS EC2, DigitalOcean.

### Telepítési modellek (Deployment Models)
* **Public (Nyilvános):** Bárki számára elérhető (pl. Gmail). Olcsó, de kevésbé biztonságos.
* **Private (Magán):** Egyetlen szervezet számára fenntartott felhő. Drága, de nagyon biztonságos (pl. banki belső felhő).
* **Hybrid (Hibrid):** A kettő keveréke. Az érzékeny adatokat privátban tartják, a többit a nyilvánosban.
* **Community (Közösségi):** Hasonló célú szervezetek (pl. egyetemek, kutatóintézetek) osztják meg egymással az erőforrásokat.

---

## 9.3 A felhő 5 jellemzője (NIST szerint)
1.  **On-demand self-service:** Önkiszolgáló (kattintással rendelhetsz szervert).
2.  **Broad network access:** Bárhonnan elérhető hálózaton.
3.  **Resource pooling:** Erőforrás-megosztás (több felhasználó osztozik a hardveren).
4.  **Rapid elasticity:** Gyors rugalmasság (ha kell, automatikusan több erőforrást kapsz).
5.  **Measured service:** Mérhető szolgáltatás (annyit fizetsz, amennyit használsz - "Pay-as-you-go").

---

## 9.4 OSZTV Ellenőrző kérdések (Bank alapján)

**1. Melyik felhőmodell biztosít fejlesztői környezetet és eszközöket programozóknak?**
* *Válasz: PaaS (Platform as a Service).*

**2. Melyik hypervisor típus települ közvetlenül a hardverre?**
* *Válasz: Type 1 (Bare-metal).*

**3. Mi a Dropbox vagy a Gmail szolgáltatási modellje?**
* *Válasz: SaaS (Software as a Service).*

**4. Egy cég központi szerveren szeretné futtatni a felhasználók Windows asztalait. Milyen technológiát kell használniuk?**
* *Válasz: VDI (Virtual Desktop Infrastructure).*

**5. Melyik telepítési modell esetén használják közösen az infrastruktúrát hasonló érdeklődésű szervezetek?**
* *Válasz: Community Cloud (Közösségi felhő).*

---

# 10. Fejezet: Windows telepítés (ITE)

Ez a fejezet a Windows telepítését, a fájlrendszereket, a lemezkezelést és a rendszerindítási folyamatot (Boot sequence) tárgyalja.

---

## 10.1 Operációs rendszer alapok

### Fogalmak
* **GUI (Graphical User Interface):** Grafikus felhasználói felület (egérrel, ikonokkal vezérelhető).
* **CLI (Command Line Interface):** Parancssoros felület (parancsokat kell begépelni).
* **Multi-tasking:** Több alkalmazás futtatása egyszerre.
* **Multi-threading (Többszálúság):** Egy program kódjának több része fut egyszerre.
* **Multi-processing:** Több CPU (vagy mag) használata egyszerre.

### Architektúrák (32 vs 64 bit)
* **32-bit (x86):** Maximum **4 GB RAM**-ot tud megcímezni (kezelni).
* **64-bit (x64):** 128 GB+ RAM-ot kezel, és gyorsabb adatfeldolgozást tesz lehetővé.

---

## 10.2 Lemezkezelés és Fájlrendszerek

### Partíciós táblák
1.  **MBR (Master Boot Record):**
    * Régi szabvány.
    * Maximum **4 elsődleges** (Primary) partíció.
    * Maximum **2 TB** lemezméret.
2.  **GPT (GUID Partition Table):**
    * Új szabvány (UEFI-hez).
    * Akár 128 partíció.
    * 2 TB-nál nagyobb lemezeket is kezel.

### Fájlrendszerek (File Systems)
* **FAT32:** Régi, kompatibilis, de max. 4 GB lehet egy fájl mérete.
* **NTFS:** A Windows alapértelmezett fájlrendszere. Támogatja a **fájlméret-korlát nélküliséget**, a **jogosultságokat**, a tömörítést és a titkosítást.
* **exFAT:** USB meghajtókhoz, FAT32 utódja (nincs 4GB korlát), de nincs NTFS biztonság.

### Lemez típusok
* **Basic (Alap) lemez:** Hagyományos partíciók (elsődleges, kiterjesztett).
* **Dynamic (Dinamikus) lemez:** Lehetővé teszi a **kötetek kiterjesztését** több fizikai lemezre (Spanning), vagy szoftveres RAID (Tükrözés/Striping) létrehozását.

---

## 10.3 Telepítési módszerek

* **Clean Install (Tiszta telepítés):** Üres lemezre, vagy formázás után.
* **Upgrade (Frissítés):** A régi Windows fájljai és beállításai megmaradnak (pl. Win 8 -> Win 10).
* **Unattended (Felügyelet nélküli):** Egy válaszfájl (unattend.xml) tartalmazza a kérdésekre a választ, így nem kell a gép előtt ülni.
* **PXE (Preboot eXecution Environment):** Hálózati telepítés. A gép a hálózati kártyáról bootol, és egy szerverről tölti be a telepítőt.
* **Image (Lemezkép) telepítés / Klónozás:** Egy készre telepített gép másolása sok másikra.
    * **Sysprep:** Fontos eszköz klónozás előtt! Eltávolítja az egyedi azonosítókat (SID), hogy a klónozott gépek ne ütközzenek a hálózaton.
* **USMT (User State Migration Tool):** Parancssoros eszköz felhasználói adatok átköltöztetésére nagyvállalati környezetben.

---

## 10.4 A Windows Boot folyamata (OSZTV KRITIKUS!)

Ezt a sorrendet (főleg a fájlneveket) gyakran kérik:

1.  **POST:** Hardverellenőrzés.
2.  **BIOS/UEFI:** Megkeresi a boot eszközt.
3.  **Windows Boot Manager (bootmgr):** Átveszi az irányítást a lemezen.
4.  **Winload.exe:** Betölti a rendszermagot.
5.  **Ntoskrnl.exe:** Ez a Windows Kernel (rendszermag).
6.  **Hal.dll:** Hardware Abstraction Layer (kapcsolat a hardverrel).
7.  **Winlogon.exe:** Megjeleníti a bejelentkező képernyőt.

---

## 10.5 Helyreállítás és Indítási módok

* **F8 billentyű:** Rendszerindításkor ezzel érhető el a "Speciális rendszerindítási lehetőségek" (Advanced Boot Options).
* **Safe Mode (Csökkentett mód):** Csak a legszükségesebb drivereket tölti be. Hibaelhárításra (pl. vírusirtás, rossz driver törlése) használjuk.
* **Last Known Good Configuration (Legutolsó helyes konfiguráció):** A legutóbbi sikeres bejelentkezéskori Registry-t állítja vissza. Akkor jó, ha egy driver telepítése után nem indul a gép.
* **Restore Point (Visszaállítási pont):** Pillanatkép a rendszer állapotáról.

---

## 10.6 OSZTV Ellenőrző kérdések (Bank alapján)

**1. Melyik billentyűvel érhető el a Csökkentett mód (Safe Mode) indításkor?**
* *Válasz: F8.*

**2. Mekkora a maximális RAM, amit egy 32 bites Windows kezelni tud?**
* *Válasz: 4 GB.*

**3. Melyik eszköz (tool) segítségével távolíthatjuk el az egyedi azonosítókat (SID) klónozás előtt?**
* *Válasz: Sysprep.*

**4. Mi a teendő, ha a gép "Invalid Boot Disk" hibát ír ki indításkor?**
* *Válasz: Ellenőrizni kell a BIOS-ban a Boot sorrendet (pl. nincs-e benne felejtve egy pendrive) vagy sérült az MBR.*

**5. Melyik fájlrendszert kell használni Windows telepítéshez?**
* *Válasz: NTFS.*

**6. Melyik technológia teszi lehetővé a hálózaton keresztüli telepítést (indítást)?**
* *Válasz: PXE.*

**7. Mi a dinamikus lemez (Dynamic Disk) előnye?**
* *Válasz: Kötetek kiterjesztése több fizikai lemezre (Spanning).*

---

# 11. Fejezet: Windows konfiguráció (ITE)

Ez a fejezet a Windows mélyreható beállításait, a felügyeleti eszközöket és a parancssori (CLI) parancsokat tárgyalja.

---

## 11.1 Windows Asztal és Eszközök

* **Tálca (Taskbar):** A futó programok és rögzített ikonok helye.
* **Értesítési terület (System Tray):** Az óra melletti ikonok (hálózat, hangerő).
* **Feladatkezelő (Task Manager):**
    * *Megnyitás:* `Ctrl+Shift+Esc` vagy `Ctrl+Alt+Del`.
    * *Fülek:*
        * **Processes:** Futó programok és erőforrás-használat.
        * **Performance:** CPU, Memória, Lemez grafikonok.
        * **Startup:** Rendszerindításkor automatikusan induló programok (Win 8/10/11).
        * **Services:** Háttérben futó szolgáltatások.

* **Fájlkezelő (File Explorer):**
    * **Fájlkiterjesztések:** A fájl típusát jelölik (pl. .txt, .exe, .jpg). Alapértelmezésben rejtettek!
    * **Attribútumok:**
        * **R (Read-only):** Csak olvasható.
        * **H (Hidden):** Rejtett.
        * **S (System):** Rendszerfájl (kritikus).
        * **A (Archive):** Archiválandó (mentéshez jelzi, hogy megváltozott).

---

## 11.2 Vezérlőpult (Control Panel) kategóriák

* **System and Security:** Tűzfal, BitLocker, Windows Update, Biztonsági mentés.
* **Network and Internet:** Hálózati adapterek, megosztás, Otthoni csoport.
* **Hardware and Sound:** Eszközkezelő, Nyomtatók, Hangerő.
* **User Accounts:** Felhasználók kezelése, Jelszavak.
* **Programs:** Programok telepítése/törlése.

### Fontos segédprogramok
* **Device Manager (Eszközkezelő):** Hardverek és illesztőprogramok (driverek) kezelése.
    * *Sárga háromszög:* Hiba vagy hiányzó driver.
    * *Lefelé mutató nyíl:* Letiltott eszköz.
* **Disk Management (Lemezkezelés):** Particionálás, formázás, kötetek kezelése.

---

## 11.3 Parancssori eszközök (CLI) - OSZTV KRITIKUS!

Ezeket a parancsokat tudni kell (funkció és kapcsolók):

### Fájlrendszer
* **cd (change directory):** Könyvtárváltás. `cd ..` (visszalépés).
* **dir:** Könyvtár tartalmának listázása.
* **md (make directory):** Új mappa létrehozása.
* **rd (remove directory):** Mappa törlése.
* **copy / xcopy / robocopy:** Fájlok másolása (a robocopy a legfejlettebb).
* **del:** Fájl törlése.

### Lemezkezelés
* **chkdsk (Check Disk):** Fájlrendszer hibák ellenőrzése. `chkdsk /f` (javítás), `chkdsk /r` (szektorhiba javítása).
* **format:** Meghajtó formázása.
* **diskpart:** Fejlett lemezkezelő parancssori környezet (partíciók létrehozása, törlése).

### Hálózat (Nagyon fontos!)
* **ipconfig:** IP cím megjelenítése. `ipconfig /all` (minden adat, MAC cím is). `ipconfig /release` & `/renew` (új IP kérése).
* **ping:** Kapcsolat tesztelése (ICMP).
* **tracert:** Útvonalkövetés (hol szakad meg a net?).
* **nslookup:** DNS névfeloldás tesztelése (név -> IP).
* **netstat:** Aktív hálózati kapcsolatok listázása.

### Rendszer
* **sfc /scannow (System File Checker):** Sérült rendszerfájlok keresése és javítása.
* **gpupdate /force:** Csoportszabályok (Group Policy) azonnali frissítése.
* **shutdown:** Gép kikapcsolása. `shutdown /s /t 0` (azonnal), `shutdown /r` (újraindítás).
* **tasklist / taskkill:** Futó folyamatok listázása és leállítása.

---

## 11.4 Hálózatkezelés és Megosztás

* **Munkacsoport (Workgroup):** Egyenrangú gépek hálózata (Peer-to-Peer). Nincs központi felügyelet.
* **Tartomány (Domain):** Központi szerver (Domain Controller) irányítja a gépeket és a felhasználókat (Active Directory).
* **Megosztás (Sharing):**
    * *Mapping a drive (Hálózati meghajtó csatlakoztatása):* Egy távoli megosztott mappát betűjellel látunk el (pl. Z: meghajtó).
* **VPN (Virtual Private Network):** Biztonságos, titkosított csatorna az interneten keresztül a céges hálózatba.
* **Remote Desktop (RDP):** Távoli asztal elérés (grafikus felületen).

---

## 11.5 OSZTV Ellenőrző kérdések (Bank alapján)

**1. Melyik parancs listázza ki az aktuális könyvtár tartalmát?**
* *Válasz: dir.*

**2. Melyik paranccsal ellenőrizhetjük a hálózati kapcsolatot egy távoli szerverrel?**
* *Válasz: ping.*

**3. Melyik eszközben láthatjuk a hiányzó illesztőprogramokat (sárga felkiáltójel)?**
* *Válasz: Eszközkezelő (Device Manager).*

**4. Melyik parancs javítja a fájlrendszer logikai hibáit?**
* *Válasz: chkdsk /f.*

**5. Hol lehet beállítani, hogy mely programok induljanak el automatikusan a Windows 10 indításakor?**
* *Válasz: Feladatkezelő (Task Manager) -> Indítás (Startup) fül.*

**6. Melyik paranccsal kérhetünk új IP-címet a DHCP szervertől?**
* *Válasz: ipconfig /renew.*

---

# 12. Fejezet: Mobil, Linux és macOS operációs rendszerek (ITE)

Ez a fejezet a Windows alternatíváit és a mobileszközöket tárgyalja. Az OSZTV-n a Linux fájljogosultságok és a parancssor ismerete kiemelt jelentőségű.

---

## 12.1 Mobileszközök Operációs Rendszerei

### Android vs iOS
* **Android (Google):**
    * **Nyílt forráskódú (Open Source):** Bárki módosíthatja (ezért van annyi gyártói felület, pl. Samsung One UI, Xiaomi MIUI).
    * **Alkalmazások:** Google Play Store (de külső forrásból is telepíthető APK fájl).
    * **Fájlrendszer:** Hozzáférhető, mappákba rendezhető.
    * **Vissza gomb:** Fizikai vagy szoftveres gomb a visszalépéshez.
* **iOS (Apple):**
    * **Zárt forráskódú (Closed Source):** Csak az Apple fejleszti és csak Apple eszközökön fut.
    * **Alkalmazások:** App Store (szigorúan ellenőrzött, "Walled Garden" modell).
    * **Fájlrendszer:** Zárt, alkalmazásonként elkülönített (Sandbox).
    * **Home gomb:** (Régebbi modelleken) ez visz a főképernyőre.

### Mobil biztonság és kifejezések
* **Rooting (Android):** Rendszergazdai (root) jogok megszerzése. Veszélyes, de teljes kontrollt ad.
* **Jailbreaking (iOS):** Az Apple korlátozásainak feltörése, hogy nem hivatalos appokat lehessen telepíteni.
* **Sandbox (Homokozó):** Az alkalmazások elszigetelt környezetben futnak, nem férnek hozzá egymás adataihoz vagy a rendszerhez engedély nélkül.
* **Remote Wipe (Távoli törlés):** Lopás esetén az eszköz teljes tartalmának törlése távolról (iCloud / Google Find My Device).

---

## 12.2 Linux és macOS alapok

### macOS (Apple)
* **Alap:** UNIX alapú rendszer (stabil, biztonságos).
* **Felület:** Aqua GUI.
* **Eszközök:**
    * **Finder:** Fájlkezelő (mint a Windows Intéző).
    * **Time Machine:** Beépített biztonsági mentés.
    * **Spotlight:** Kereső.
    * **Dock:** Tálca az ikonokkal.
    * **Force Quit:** Program kényszerített bezárása (mint a Feladatkezelőben a "Feladat befejezése").

### Linux
* **Alap:** Nyílt forráskódú kernel (Linus Torvalds).
* **Disztribúciók (Distro):** Különböző csomagok (pl. Ubuntu, Debian, Fedora, Red Hat).
* **Fájlrendszer:** **ext3** vagy **ext4** (Journaling file system - naplózó fájlrendszer, áramszünet esetén véd az adatvesztéstől).

---

## 12.3 Linux Parancssor (CLI) - OSZTV KRITIKUS!

Ezeket a parancsokat és a jogosultságokat (rwx) **tudni kell**!

### Alapvető parancsok
* **ls:** Listázás (mint a `dir` Windowsban).
    * `ls -l`: Részletes lista (jogosultságok, tulajdonos, méret).
* **cd:** Könyvtárváltás.
* **mkdir:** Új könyvtár.
* **cp:** Másolás (copy).
* **mv:** Áthelyezés (move) vagy átnevezés.
* **rm:** Törlés (remove). `rm -r`: Könyvtár törlése tartalommal együtt.
* **pwd:** Jelenlegi könyvtár útvonala (Print Working Directory).
* **chmod:** Jogosultságok módosítása (Change Mode).
* **chown:** Tulajdonos módosítása (Change Owner).
* **sudo:** Parancs futtatása rendszergazdaként (SuperUser Do).
* **vi / vim / nano:** Szövegszerkesztők terminálban.
* **grep:** Keresés fájl tartalmában.
* **ifconfig:** Hálózati beállítások (mint az `ipconfig` Windowsban). **Újabb rendszereken:** `ip addr`.

### Jogosultságok (Permissions) - `rwx`
A Linux fájlok jogait 3 csoportra osztjuk: **User (u)**, **Group (g)**, **Others (o)**.
Minden csoporthoz 3 jog tartozik:
* **r (read - 4):** Olvasás.
* **w (write - 2):** Írás.
* **x (execute - 1):** Futtatás.

**Példa:** `drwxr-xr--`
1.  **d:** Directory (könyvtár). Ha `-`, akkor fájl.
2.  **rwx (7):** A tulajdonos (User) olvashatja, írhatja, futtathatja. (4+2+1=7)
3.  **r-x (5):** A csoport (Group) olvashatja és futtathatja. (4+0+1=5)
4.  **r-- (4):** Mindenki más (Others) csak olvashatja. (4+0+0=4)
* *Numerikus alak:* **754**

---

## 12.4 Karbantartás és Helyreállítás

* **Linux mentés:** `tar` (tömörítés), `rsync` (szinkronizálás).
* **Frissítés:** Csomagkezelőkkel (Package Manager).
    * Debian/Ubuntu: `apt-get update` & `apt-get upgrade`.
    * RedHat/Fedora: `yum` vagy `dnf`.
* **Cron:** Időzített feladatok futtatása (mint a Windows Feladatütemező). A szolgáltatás neve: `crond`.

---

## 12.5 OSZTV Ellenőrző kérdések (Bank alapján)

**1. Melyik Linux fájlrendszer rendelkezik naplózási (journaling) funkcióval?**
* *Válasz: ext3 vagy ext4.*

**2. Mit jelent a "rw-r--r--" jogosultság?**
* *Válasz: A tulajdonos írhat/olvas, a csoport és a többiek csak olvashatnak.*

**3. Melyik paranccsal lehet megváltoztatni egy fájl jogosultságait?**
* *Válasz: chmod.*

**4. Mi a neve a macOS biztonsági mentési eszközének?**
* *Válasz: Time Machine.*

**5. Melyik gomb hiányzik az Android fizikai gombjai közül, ami az iOS-en (régen) megvolt?**
* *Válasz: Home gomb (Androidon szoftveres ikonok vannak).*

**6. Melyik parancs mutatja meg a jelenlegi munkakönyvtárat Linuxban?**
* *Válasz: pwd.*

---

# 13. Fejezet: Biztonság (ITE)

Ez a fejezet a kiberbiztonsági fenyegetéseket, a támadási típusokat és a védekezési stratégiákat tárgyalja. Az OSZTV-n ez az egyik legtöbb pontot érő témakör.

---

## 13.1 Kártevők (Malware) típusai

A "Malware" (Malicious Software) gyűjtőfogalom minden kártékony szoftverre.

* **Vírus (Virus):** Olyan program, ami más fájlhoz csatolja magát, és emberi beavatkozás (pl. fájl megnyitása) kell a terjedéséhez.
* **Féreg (Worm):** Önálló program, ami hálózaton keresztül terjed anélkül, hogy a felhasználónak bármit tennie kellene. Lassítja a hálózatot.
* **Trójai (Trojan):** Hasznos programnak álcázza magát (pl. játék, képernyővédő), de a háttérben kárt okoz vagy hátsó kaput nyit.
* **Ransomware (Zsarolóvírus):** Titkosítja a felhasználó adatait, és váltságdíjat követel a feloldásért.
* **Spyware (Kémprogram):** A felhasználó tudta nélkül gyűjt adatokat (jelszavak, böngészési szokások).
* **Adware:** Kéretlen reklámokat jelenít meg.
* **Rootkit:** Mélyen beépül az operációs rendszerbe, elrejti magát és más kártevőket a vírusirtók elől.

---

## 13.2 Támadási típusok

### Social Engineering (Megtévesztés)
Nem technikai támadás, hanem az emberi hiszékenység kihasználása.
* **Phishing (Adathalászat):** Megtévesztő e-mail, ami banki oldalra vagy bejelentkező felületre irányít, hogy ellopja az adatokat.
* **Vishing (Voice Phishing):** Telefonos adathalászat.
* **Shoulder Surfing:** Valaki a hátad mögött állva lesi le a jelszavadat.
* **Dumpster Diving:** Szemétben turkálás kidobott dokumentumok (jelszavak, számlák) után.
* **Tailgating / Piggybacking:** Egy illetéktelen személy besurran egy belépőkártyás ajtón egy jogosult dolgozó mögött.

### Hálózati támadások
* **DoS (Denial of Service):** Túlterheléses támadás. Annyi kérést küldenek egy szervernek, hogy az megbénul és nem tudja kiszolgálni a valódi felhasználókat.
* **DDoS (Distributed DoS):** Elosztott támadás. Sok ezer fertőzött gép (**Botnet / Zombie**) támad egyszerre egy célpontot.
* **Man-in-the-Middle (MitM):** A támadó beékelődik a kommunikációba a két fél közé, és lehallgatja vagy módosítja az adatokat.
* **Spoofing:** A támadó másnak adja ki magát (pl. hamis IP cím vagy e-mail feladó).
* **Zero-day (Nulladik napi):** Olyan sebezhetőség, amit a fejlesztők még nem ismernek, így nincs rá javítás.

---

## 13.3 Védekezési eljárások

### Adatvédelem és Titkosítás
* **BitLocker:** A teljes merevlemez titkosítása (Windows Pro/Enterprise). Használatához **TPM chip** szükséges az alaplapon.
* **EFS (Encrypting File System):** Fájlok és mappák szintű titkosítás (felhasználónként).
* **Adatmegsemmisítés:**
    * *Low-level format:* Teljes törlés.
    * *Degaussing (Lemágnesezés):* Mágneses adathordozók (HDD, szalag) végleges törlése erős mágneses térrel.
    * *Fizikai megsemmisítés:* Zúzás, fúrás (a legbiztosabb módszer).

### Hálózati védelem
* **Tűzfal (Firewall):** Szűri a bejövő és kimenő forgalmat szabályok alapján.
    * *Packet Filtering:* Csomagszűrő (IP és port alapján).
    * *Stateful Inspection:* Állapottartó (figyeli a kapcsolat állapotát).
* **WPA2:** A jelenlegi legerősebb Wi-Fi titkosítási szabvány (AES titkosítással). (A WEP és WPA elavult!)
* **SSID Broadcast tiltása:** A hálózat nevének elrejtése (nem biztonsági megoldás, csak "security through obscurity").
* **MAC Address Filtering:** Csak a listán szereplő eszközök csatlakozhatnak (könnyen kijátszható MAC spoofinggal).

### Felhasználói jogosultságok
* **Principle of Least Privilege (Legkisebb jogosultság elve):** Mindenkinek csak annyi joga legyen, amennyi a munkájához feltétlenül szükséges.

---

## 13.4 OSZTV Ellenőrző kérdések (Bank alapján)

**1. Mi a jellemzője a féreg (Worm) kártevőnek?**
* *Válasz: Önmagát sokszorosítja és hálózaton terjed emberi beavatkozás nélkül.*

**2. Milyen támadás az, amikor a támadó a szemétben keres bizalmas dokumentumokat?**
* *Válasz: Dumpster Diving.*

**3. Melyik Windows funkció titkosítja a teljes merevlemezt?**
* *Válasz: BitLocker.*

**4. Mi szükséges a BitLocker használatához az alaplapon?**
* *Válasz: TPM (Trusted Platform Module) chip.*

**5. Mi a teendő, ha egy számítógépen illegális tartalmat (pl. gyermekpornográfia) találsz javítás közben?**
* *Válasz: Azonnal leállítani a munkát, nem szólni az ügyfélnek, és értesíteni a hatóságokat (First Responder).*

**6. Melyik a legerősebb Wi-Fi titkosítási protokoll a felsoroltak közül (WEP, WPA, WPA2)?**
* *Válasz: WPA2.*

**7. Mi a DDoS támadás célja?**
* *Válasz: A szolgáltatás megbénítása (Denial of Service) túlterheléssel.*

**8. Mi a Botnet?**
* *Válasz: Fertőzött számítógépek (Zombik) hálózata, amit a támadó távolról irányít.*

---

# 14. Fejezet: Az informatikai szakértő (ITE)

Ez a fejezet a kommunikációs készségeket, a professzionális viselkedést, a jogi szabályozásokat és az alapvető szkriptelést (scripting) tárgyalja.

---

## 14.1 Kommunikáció és Professzionális Magatartás

### Kommunikációs stílusok
* **Level 1 (Első szintű) technikus:**
    * Feladata: Adatgyűjtés az ügyféltől, a probléma rögzítése (ticket), egyszerű hibák (jelszócsere, toner) megoldása.
    * Ha nem tudja megoldani, továbbítja (eszkalálja) a 2. szintre.
* **Level 2 (Második szintű) technikus:**
    * Feladata: Bonyolultabb hibák megoldása (távoli asztallal, diagnosztikai szoftverrel). Ő hívja vissza az ügyfelet a Level 1 eszkalációja után.

### Ügyfélkezelési szabályok (Netikett)
1.  **Aktív hallgatás:** Hagyd az ügyfelet beszélni, ne szakítsd félbe (kivéve, ha irányítani kell a beszélgetést). Használj megerősítéseket ("Értem", "Igen").
2.  **Szakzsargon kerülése:** Beszélj érthetően, ne használj technikai rövidítéseket (pl. "DHCP", "DNS"), ha az ügyfél nem ért hozzá.
3.  **Nehéz ügyfelek:**
    * *Beszédes:* Tegyél fel zárt kérdéseket (Igen/Nem válasz), hogy visszavedd az irányítást.
    * *Dühös:* Ne vedd magadra, kérj elnézést a kellemetlenségért (még ha nem is te okoztad), és fókuszálj a megoldásra.
    * *Tapasztalatlan:* Légy türelmes, lépésről lépésre magyarázz.

---

## 14.2 Jogi és Etikai Szabályozások

### Adatvédelem és Megfelelőség (Compliance)
* **PII (Personally Identifiable Information):** Személyes azonosításra alkalmas adat (pl. név, lakcím, TB-szám). Szigorúan védendő!
* **PHI (Protected Health Information):** Egészségügyi adatok (pl. kórházi leletek). (USA-ban: HIPAA törvény).
* **PCI (Payment Card Industry):** Bankkártyaadatok védelme.
* **GDPR (General Data Protection Regulation):** Az EU általános adatvédelmi rendelete.

### Számítógépes bűnözés és bizonyítékok (Forensics)
* **Cyber Law (Kibertörvény):** A digitális bűncselekményekre vonatkozó jogszabályok.
* **First Responder (Elsődleges beavatkozó):** Az a személy, aki először érkezik a helyszínre és biztosítja a bizonyítékokat.
* **Chain of Custody (Felügyeleti lánc):** Dokumentáció arról, hogy ki, mikor, hogyan nyúlt a bizonyítékhoz. Ha megszakad, a bizonyíték érvénytelen a bíróságon!
* **Volatile Data (Illékony adat):** Olyan adat, ami áramtalanításkor elvész (RAM, CPU cache, regiszterek). Ezt kell először menteni!

### Licencelés
* **Personal License:** Egy gépre, otthoni használatra.
* **Enterprise / Site License:** Vállalati licenc, korlátlan vagy sok gépre telepíthető.
* **Open Source (Nyílt forráskód):** A forráskód megtekinthető, módosítható és terjeszthető (pl. Linux, LibreOffice). Nem feltétlenül ingyenes, de a kód szabad.

---

## 14.3 Szkriptelés (Scripting)

A szkriptek egyszerű szöveges fájlok, amik parancsokat tartalmaznak a feladatok automatizálására. Nem kell őket lefordítani (compiler), soronként hajtódnak végre (interpreter).

### Szkriptnyelvek típusai
* **Windows Batch (.bat):** Régi, egyszerű parancssori szkriptek.
* **PowerShell (.ps1):** Modern, objektum-orientált, nagyon erős Windows adminisztrációs eszköz.
* **VBScript (.vbs):** Régebbi Windows szkriptnyelv.
* **Shell Script (.sh):** Linux/Unix rendszereken használt (Bash).
* **Python (.py):** Univerzális, könnyen olvasható nyelv.
* **JavaScript (.js):** Webes és kliensoldali szkriptek.

### Alapfogalmak
* **Változó (Variable):** Adatok tárolására szolgáló hely (pl. `x = 5`).
* **Ciklus (Loop):** Ismétlődő feladatok (pl. `for`, `while`).
* **Feltétel (Conditional):** Elágazások (pl. `if-then-else`).

---

## 14.4 OSZTV Ellenőrző kérdések (Bank alapján)

**1. Mi a Level 1 (első szintű) technikus elsődleges feladata?**
* *Válasz: Információgyűjtés az ügyféltől és a hibajegy (ticket) rögzítése.*

**2. Mit kell tenned, ha illegális tartalmat találsz a gépen?**
* *Válasz: Jelenteni a megfelelő csatornákon (felettesnek, hatóságnak), és nem szólni az ügyfélnek.*

**3. Melyik adattípust kell először menteni egy nyomozás során?**
* *Válasz: Az illékony adatokat (Volatile Data), pl. a RAM tartalmát.*

**4. Mi a Chain of Custody (Felügyeleti lánc) célja?**
* *Válasz: A bizonyítékok hitelességének és sértetlenségének igazolása a bíróság előtt (hogy nem manipulálták).*

**5. Melyik szkriptnyelv végződése a .ps1?**
* *Válasz: PowerShell.*

**6. Melyik programozási nyelvtípus igényel fordítót (compiler) a futtatás előtt (pl. C++, Java), szemben a szkriptekkel?**
* *Válasz: A lefordított (compiled) nyelvek.*

**7. Mi a teendő egy bőbeszédű ügyféllel?**
* *Válasz: Udvariasan átvenni az irányítást zárt kérdésekkel (amire csak igennel vagy nemmel lehet felelni).*
