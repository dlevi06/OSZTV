# ☁️ 1. Modul: Felhőtechnológiák (Cloud Computing)

*A versenyen általában Azure és AWS alapfogalmakat kérdeznek vegyesen.*

### Alapfogalmak

* **Definíció:** Interneten keresztül nyújtott számítástechnikai szolgáltatások (szerver, tároló, adatbázis, szoftver).
* **Pay-as-you-go:** Használatalapú fizetés (csak azért fizetsz, amit használsz, nincs beruházási költség).
* **Skálázhatóság (Scalability):** Az erőforrások növelése (vagy csökkentése) az igények szerint.
* *Scale Up:* Erősebb vas (több RAM/CPU) ugyanabba a gépbe.
* *Scale Out:* Több gép beállítása egymás mellé.


* **On-premises:** Hagyományos, saját telephelyen üzemeltetett szerverpark (saját vas).

### Szolgáltatási Modellek (Ezt mindig kérdezik!)

1. **IaaS (Infrastructure as a Service) - Infrastruktúra:**
* A szolgáltató adja a "vasat" (szerver, hálózat, tároló), virtualizációt.
* Te telepíted az Operációs Rendszert és a szoftvereket.
* *Példa:* Amazon EC2, Azure VM (Virtual Machine), Google Compute Engine.


2. **PaaS (Platform as a Service) - Platform:**
* A szolgáltató adja az OS-t és a fejlesztői környezetet is. Te csak a kódot viszed.
* *Példa:* Google App Engine, Azure App Service, Heroku.


3. **SaaS (Software as a Service) - Szoftver:**
* Kész szoftver használata böngészőből. Semmit nem kell telepítened vagy üzemeltetned.
* *Példa:* Google Drive, Microsoft 365 (Office), Gmail, Dropbox.



### Telepítési Modellek

* **Public Cloud (Nyilvános):** Bárki bérelhet (pl. AWS, Azure). Olcsóbb, de megosztott erőforrás.
* **Private Cloud (Privát):** Egyetlen szervezet kizárólagos használatára (biztonságosabb, de drágább).
* **Hybrid Cloud:** A kettő kombinációja (pl. érzékeny adat a privátban, weboldal a publikusban).

### Specifikus Szolgáltatások (AWS & Azure szótár)

* **AWS EC2:** Virtuális szerver bérlés (Compute).
* **AWS S3:** Fájltárolás, objektumtároló (Storage).
* **Azure VM:** Virtuális gép (ugyanaz, mint az EC2).
* **Azure Entra ID (régen Active Directory):** Felhős felhasználó- és jogosultságkezelés.
* **Régiók (Regions):** Földrajzi helyek, ahol az adatközpontok vannak. (Minél közelebb van, annál kisebb a késleltetés).

---

# 🐧 2. Modul: Linux Rendszergazda Alapok

*A versenyen parancsokat és jogosultságokat kérdeznek.*

### Alap parancsok

* `pwd`: Hol vagyok most? (Print Working Directory).
* `ls`: Listázás (könyvtár tartalma). `ls -l` (részletes), `ls -a` (rejtett fájlok is).
* `cd`: Könyvtárváltás (`cd ..` = egyet feljebb lép).
* `cp [mit] [hova]`: Másolás (Copy).
* `mv [mit] [hova]`: Áthelyezés vagy Átnevezés (Move).
* `rm [fájl]`: Törlés (Remove).
* `rm -r`: Könyvtár törlése (rekurzív).
* `rm *`: Minden fájl törlése.


* `mkdir`: Új könyvtár (Make Directory).
* `grep "szöveg" [fájl]`: Keresés fájl tartalmában.
* `cat [fájl]`: Fájl tartalmának kiíratása.
* `sudo`: Rendszergazdai (root) joggal futtatás.

### Jogosultságok (chmod) - Nagyon fontos!

A jogok 3 csoportra oszlanak: **User** (tulaj), **Group** (csoport), **Others** (többiek).
Jelölés: **r** (read - 4), **w** (write - 2), **x** (execute/futtatás - 1).

* **Matematika:**
* `chmod 777`: Mindenkinek teljes jog (4+2+1 = 7).
* `chmod 755`: Tulajdonos ír/olvas/futtat (7), többiek csak olvas/futtat (5).
* `chmod +x [fájl]`: Futtatási jogot ad a fájlnak (ez a leggyakoribb kérdés!).



### Rendszerkarbantartás

* **Csomagkezelés (Debian/Ubuntu):**
* `sudo apt update`: Csomaglisták frissítése (nem telepít, csak listát frissít!).
* `sudo apt upgrade`: A telepített programok frissítése új verzióra.


* **Szkriptek:**
* Kiterjesztés: `.sh`
* Első sor (Shebang): `#!/bin/bash` (ez mondja meg, mivel kell futtatni).


* **Időzítés:** `cron` (időzített feladatok futtatása).
* **Csatolás:** Pendrive-ot vagy lemezt általában a `/mnt` vagy `/media` mappába csatolunk (mount).

---

# 🪟 3. Modul: Windows és PowerShell

*Főleg szerveres és parancssoros kérdések.*

### PowerShell (PS)

* **Kiterjesztés:** `.ps1`
* **Szintaxis:** Ige-Főnév (Verb-Noun). Pl.: `Get-Service`, `New-Item`.
* `New-ADUser`: Új felhasználó létrehozása Active Directoryban.
* `ipconfig`: IP cím lekérdezése (`/all`, `/release`, `/renew`).

### Rendszer

* **Active Directory (AD):** Központi címtár (felhasználók, gépek kezelése).
* **GPO (Group Policy):** Csoportházirend. Ezzel tiltunk/engedélyezünk dolgokat a klienseken központilag (pl. háttérkép, USB tiltás). **BIOS-t NEM lehet vele állítani!**
* **Rendszergazda csoport:** `Administrators`.
* **Fájlrendszerek:**
* **NTFS:** A szabvány Windows fájlrendszer (jogosultságok, kvóták).
* **ReFS:** Új, szerveres fájlrendszer (hibatűrőbb), de nem bootolható.



---

# 💾 4. Modul: Adatbázis (SQL) és Git

*Csak az alapokat kérdezik.*

### SQL (Structured Query Language)

* `SELECT * FROM tabla`: Minden adat lekérése.
* `WHERE`: Szűrés (pl. `WHERE ar > 1000`).
* `UPDATE tabla SET mezo = ertek`: Adat módosítása. (Pl. áremelés: `ar = ar * 1.2`).
* `INSERT INTO`: Új adat beszúrása.
* `ALTER TABLE`: A tábla szerkezetének módosítása (pl. új oszlop hozzáadása).
* **Függvények:** `AVG()` (átlag), `COUNT()` (megszámolás).
* **Redundancia:** Adatismétlődés (ezt kerülni kell a normalizálással).

### Git (Verziókezelés)

* **Cél:** Forráskód változásainak követése, közös munka.
* **GitHub:** Felhő alapú Git szolgáltatás.
* **Parancsok:**
* `git clone`: Letöltés a szerverről (első alkalommal).
* `git pull`: Frissítések letöltése.
* `git add .`: Változások előkészítése.
* `git commit -m "üzenet"`: Változások rögzítése helyben.
* `git push`: Feltöltés a szerverre.



---

# 🌐 5. Modul: Web (HTML/CSS)

*A rendszerüzemeltetőktől csak a kód olvasását várják el.*

* **Alapszerkezet:** `<html>`, `<head>` (meta adatok, CSS, cím), `<body>` (látható tartalom).
* **Fontos Tagek:**
* `<a href="url">`: Link (Hivatkozás).
* `<img src="kep.jpg" alt="leírás">`: Kép. (Az `alt` jelenik meg, ha nincs kép).
* `<input type="text">`: Beviteli mező.
* `<script>`: JavaScript kód helye.
* `<link rel="stylesheet">`: Külső CSS fájl csatolása.


* **Bootstrap:** Egy CSS keretrendszer.
* **Grid system:** 12 oszlopra osztja a képernyőt.
* `col-md-4`: Közepes képernyőn 4 egység széles (tehát 3 ilyen fér el egymás mellett).
* Osztályok: `.container`, `.row`.



---
