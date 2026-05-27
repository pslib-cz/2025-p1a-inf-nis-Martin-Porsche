# Technická dokumentace – Katalog požadavků softwarového projektu
**Projekt:** RetroKatalog – Katalog a tracker retro konzolí a her   
**Datum:** 27. 5. 2026  

---

## 1. Úvod a cíle projektu

### 1.1 Účel dokumentu
Tento dokument slouží jako kompletní katalog funkčních a nefunkčních požadavků pro informační systém **RetroKatalog**. Dokumentace je strukturována podle standardů softwarového inženýrství vyžadovaných v rámci předmětu Technická dokumentace. Slouží jako závazný podklad pro následný návrh UML diagramů (Use Case, Class, ERD) a samotnou implementaci.

### 1.2 Cíl systému
Hlavním cílem systému RetroKatalog je poskytnout sběratelům a nadšencům do historického hardwaru a softwaru robustní platformu pro evidenci, správu a sledování stavu jejich sbírek. Na rozdíl od běžných herních databází klade RetroKatalog důraz na detailní hardwarové specifikace (revize desek, modifikace, stav kondenzátorů) a propojení fyzického stavu médií s herním trackerem (backlogem).

---

## 2. Uživatelé a aktéři systému (Actors)

V systému jsou definovány následující role, které budou později modelovány v Use Case diagramu:

| Role (Aktér) | Popis a úroveň oprávnění |
| :--- | :--- |
| **Anonymní návštěvník (Guest)** | Neautentizovaný uživatel. Může pouze prohlížet veřejný globální katalog her a konzolí nebo veřejně sdílené sbírky ostatních uživatelů. Nemůže nic editovat ani zadávat data. |
| **Registrovaný sběratel (User)** | Plně autentizovaný uživatel. Má privátní profil, spravuje vlastní sbírku, eviduje svůj hardware, modifikace a herní backlog. Může navrhovat nové položky do globálního katalogu. |
| **Moderátor obsahu (Moderator)** | Uživatel s rozšířenými právy. Kontroluje a schvaluje návrhy na nové konzole a hry v globálním katalogu, čímž zajišťuje konzistenci a správnost dat. |
| **Administrátor (Admin)** | Nejvyšší úroveň oprávnění. Spravuje uživatelské účty, řeší nahlášení chyb, provádí zálohování systému a konfiguraci globálních parametrů. |

---

## 3. Funkční požadavky (Functional Requirements)

Funkční požadavky definují konkrétní chování a operace, které systém musí provádět. Jsou rozděleny do logických modulů.

### 3.1 Modul: Autentizace a správa uživatelů (F1)
* **F1.1: Registrace uživatele** – Systém umožní anonymnímu návštěvníkovi vytvořit si účet zadáním e-mailu, unikátního uživatelského jména a bezpečného hesla.
* **F1.2: Přihlášení a odhlášení** – Bezpečné ověření identity uživatele (implementace pomocí JWT nebo sessions).
* **F1.3: Nastavení soukromí sbírky** – Uživatel si může zvolit, zda je jeho sbírka zcela privátní, přístupná přes unikátní odkaz, nebo plně veřejná v žebříčcích sběratelů.
* **F1.4: Správa profilu** – Možnost změny hesla, e-mailu a profilového obrázku.

### 3.2 Modul: Globální katalog (F2)
* **F2.1: Prohlížení a filtrování** – Vyhledávání v encyklopedické části systému podle platformy (např. Sony PSP, Nintendo Game Boy, SEGA Genesis), žánru, roku vydání a vydavatele.
* **F2.2: Návrh nové položky** – Registrovaný uživatel může podat návrh na přidání chybějící hry nebo varianty konzole do globálního katalogu přes formulář.
* **F2.3: Schvalovací proces** – Moderátor má rozhraní pro schválení, zamítnutí nebo úpravu podaných návrhů.

### 3.3 Modul: Osobní sbírka Hardwaru (F3)
* **F3.1: Přidání hardwaru do sbírky** – Uživatel si naváže položku z globálního katalogu (např. PSP 3000) do své osobní skříně.
* **F3.2: Evidence technických detailů** – Možnost zadat sériové číslo, regionální variantu (PAL, NTSC-U, NTSC-J) a vizuální/funkční stav (škála 1–10).
* **F3.3: Správa hardwarových modifikací** – Možnost u daného kusu zaškrtnout a specifikovat úpravy:
    * *Softwarové:* Verze Custom Firmware (např. ARK-4 CFW, PRO-C).
    * *Hardwarové:* Typ instalovaného displeje (IPS mod), výměna mechaniky za SD emulátor (ODE), bateriové modifikace.
* **F3.4: Servisní deník** – Chronologický přehled oprav a údržby (např. „2026-05-15: Výměna termopasty a vyčištění základní desky“).

### 3.4 Modul: Osobní sbírka Softwaru a Backlog Tracker (F4)
* **F4.1: Evidence stavu média** – U každé hry ve sbírce lze určit stav balení: *Loose* (pouze cartridge/disk), *CIB* (Complete In Box – s krabičkou a manuálem) nebo *Sealed* (původní tovární fólie).
* **F4.2: Sledování herního stavu (Backlog)** – Uživatel si u hry nastavuje stav:
    * `Nehráno` – Položka pouze sedí na poličce.
    * `Rozehráno` – Aktuálně rozpracovaná hra.
    * `Dokončeno` – Hra dohrána do závěrečných titulků.
    * `Mastered (100%)` – Hra splněna na maximum (všechny achievementy/sekce).
    * `Odloženo` – Hra, která uživatele nebavila nebo ji dočasně vzdal.
* **F4.3: Záznam herního času a poznámek** – Možnost logovat odehrané hodiny a psát si osobní minirecenze.

### 3.5 Modul: Statistiky a reporty (F5)
* **F5.1: Dashboard uživatele** – Grafické zobrazení statistik (koláčový graf rozdělení her podle platforem, poměr dohraných vs. nedohraných her).
* **F5.2: Finanční přehled** – Odhadovaná hodnota sbírky na základě zadaných pořizovacích cen v porovnání s průměrnou tržní cenou.

---

## 4. Nefunkční požadavky (Non-functional Requirements)

Nefunkční požadavky definují vlastnosti, kvalitu a technologická omezení systému.

### 4.1 Výkon a odezva (Performance)
* **NF1.1: Rychlost vyhledávání** – Fulltextové vyhledávání v katalogu her musí vrátit výsledky do 200 ms při zatížení do 1000 simultánních uživatelů.
* **NF1.2: Optimalizace obrázků** – Veškeré nahrané fotografie (přebaly her, fotky hardwaru) musí být na backendu automaticky zkomprimovány a převedeny do formátu WebP pro úsporu přenosu dat.

### 4.2 Dostupnost a spolehlivost (Availability & Reliability)
* **NF2.1: Provozní dostupnost** – Cílová dostupnost webové aplikace je 99.5 % (vyjma plánovaných odstávek).
* **NF2.2: Zálohování** – Databáze systému musí být automaticky zálohována každých 24 hodin, přičemž zálohy se uchovávají po dobu 30 dnů.

### 4.3 Uživatelské rozhraní a přístupnost (Usability)
* **NF3.1: Mobile-First Design** – Rozhraní musí být plně responzivní. Důraz je kladen na mobilní zobrazení, aby uživatel mohl pohodlně ovládat systém na telefonu přímo na burzách, v bazarech nebo ve sklepě při třídění krabic.
* **NF3.2: Tmavý režim (Dark Mode)** – Vzhledem k cílové skupině (geeci, hráči) musí systém obsahovat nativní tmavý režim šetřící zrak.

### 4.4 Bezpečnost dat (Security)
* **NF4.1: Šifrování hesel** – Hesla uživatelů nesmí být nikdy uložena v plain-textu. Bude použit silný kryptografický hashovací algoritmus (např. Argon2id nebo bcrypt s dostatečným saltem).
* **NF4.2: Zabezpečený přenos** – Veškerá komunikace mezi klientem a serverem musí probíhat šifrovaně pomocí protokolu HTTPS (TLS 1.3).
* **NF4.3: Ochrana proti útokům** – Vstupy musí být sanitizovány proti SQL Injection a XSS útokům.

### 4.5 Technologická a implementační omezení (Constraints)
* **NF5.1: Backend technologie** – Node.js s využitím TypeScriptu ( framework NestJS nebo Express) pro zajištění typové bezpečnosti, což koresponduje se školní výukou moderních webových aplikací.
* **NF5.2: Databázová vrstva** – Relační databáze (PostgreSQL), z důvodu silných vazeb mezi entitami (Uživatel -> Konzole -> Specifická modifikace -> Servisní historie).
* **NF5.3: Verzování** – Zdrojový kód bude spravován v systému Git a hostován v přiděleném GitHub Classroom repozitáři.

---

## 5. Výhled na další fáze projektu
1.  **Fáze 2 (Termín do 10.6.):** Tvorba UML diagramů na základě tohoto katalogu. Bude vypracován *Use Case diagram* (zobrazení rolí a jejich akcí) a *Class/ER diagram* (struktura databázových tabulek a jejich relací – např. vztah 1:N mezi uživatelem a jeho hrami).
2.  **Fáze 3:** Finální kompletace dokumentace pro obhajobu.
