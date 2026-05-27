# Specifikace softwarových požadavků (SRS) – RetroKatalog

## 1. Úvod a cíle projektu

### 1.1 Účel dokumentu
Tento dokument definuje kompletní katalog funkčních a nefunkčních požadavků pro informační systém **RetroKatalog**. Slouží jako závazný technický podklad pro architekturu systému, návrh databázových modelů, UML diagramů a samotnou implementaci aplikace.

### 1.2 Cíl systému
Hlavním cílem systému RetroKatalog je poskytnout robustní platformu pro evidenci, správu a sledování stavu sbírek historického hardwaru a softwaru. Na rozdíl od běžných herních databází systém klade důraz na detailní technické specifikace (revize základních desek, modifikace, servisní historii) a propojení fyzického stavu médií s pokročilým herním trackerem (backlogem).

---

## 2. Uživatelé a aktéři systému (Actors)

V systému jsou definovány následující role s přesně vymezenými přístupovými právy:

| Role (Aktér) | Popis a úroveň oprávnění |
| :--- | :--- |
| **Anonymní návštěvník (Guest)** | Neautentizovaný uživatel. Má oprávnění pouze k prohlížení veřejného globálního katalogu a veřejně sdílených sbírek. Nemůže data modifikovat. |
| **Registrovaný uživatel (User)** | Autentizovaný uživatel s privátním profilem. Spravuje vlastní sbírku, eviduje osobní hardware, softwarové tituly a servisní logy. Má právo navrhovat nové položky do globálního katalogu. |
| **Moderátor (Moderator)** | Uživatel s administrativními právy pro správu obsahu. Kontroluje, upravuje a schvaluje uživatelské návrhy na rozšíření globálního katalogu. |
| **Administrátor (Admin)** | Nejvyšší úroveň oprávnění. Spravuje uživatelské účty, provádí údržbu systému, konfiguraci globálních parametrů a dohlíží na integritu databáze. |

---

## 3. Funkční požadavky (Functional Requirements)

### 3.1 Autentizace a správa uživatelů
* **F1.1: Registrace** – Systém umožní vytvoření uživatelského účtu na základě unikátního e-mailu, uživatelského jména a bezpečného hesla.
* **F1.2: Přihlášení** – Bezpečné ověření identity uživatele a generování přístupových tokenů (JWT).
* **F1.3: Správa soukromí** – Možnost nastavit viditelnost sbírky (soukromá, přístupná přes skrytý odkaz, plně veřejná).
* **F1.4: Správa profilu** – Uživatelské rozhraní pro modifikaci přístupových údajů a nastavení profilu.

### 3.2 Globální encyklopedický katalog
* **F2.1: Filtrování a vyhledávání** – Fulltextové vyhledávání v globální databázi podle platformy, regionu, žánru, roku vydání a vydavatele.
* **F2.3: Návrh obsahu** – Formulář pro zaslání požadavku na přidání nové hry nebo varianty konzole do systému.
* **F2.4: Schvalovací workflow** – Rozhraní pro moderátory k validaci a zanesení schválených návrhů do produkční databáze.

### 3.3 Evidence hardwaru
* **F3.1: Kategorizace hardwaru** – Možnost přidružit kus hardwaru z globálního katalogu do osobního vlastnictví.
* **F3.2: Technické parametry** – Evidence specifických vlastností: sériové číslo, regionální kódování (PAL, NTSC-U, NTSC-J), revize základní desky a celkový stav zařízení.
* **F3.3: Modifikace** – Sledování úprav zařízení:
    * *Softwarové:* Verze instalovaného Custom Firmwaru (CFW).
    * *Hardwarové:* Typ instalovaného zobrazovacího panelu (např. IPS mody), integrace optických emulátorů (ODE), bateriové modifikace.
* **F3.4: Servisní deník** – Chronologická evidence údržby, oprav a čištění jednotlivých hardwarových kusů.

### 3.4 Softwarová knihovna a Backlog Tracker
* **F4.1: Stav zachovalosti** – Evidence formy fyzického média (Loose / pouze médium, CIB / kompletní balení, Sealed / továrně zatavené).
* **F4.2: Stav rozpracovanosti (Backlog)** – Kategorizace herních titulů do stavů: `Nehráno`, `Rozehráno`, `Dokončeno`, `Mastered (100%)` a `Odloženo`.
* **F4.3: Telemetrie hraní** – Možnost logování odehraného času, datumu dokončení a psaní vlastních uživatelských recenzí.

### 3.5 Analytické přehledy a statistiky
* **F5.1: Uživatelský dashboard** – Vizualizace dat (poměry platforem ve sbírce, statistiky dokončených her).
* **F5.2: Finanční tracking** – Sledování pořizovacích cen položek a kalkulace celkové hodnoty sbírky.

---

## 4. Nefunkční požadavky (Non-functional Requirements)

### 4.1 Výkon a odezva
* **NF1.1: Latence vyhledávání** – Odezva fulltextového vyhledávání nesmí překročit 200 ms při modelovém zatížení 1000 simultánních požadavků.
* **NF1.2: Datová optimalizace** – Automatická komprese a konverze grafických prvků (přebaly her, fotografie stavu) do formátu WebP na straně serveru.

### 4.2 Spolehlivost a zálohování
* **NF2.1: Dostupnost systému** – Cílová dostupnost aplikační vrstvy je stanovena na 99.5 %.
* **NF2.2: Redundance dat** – Automatické zálohování databázové vrstvy v režimu 24 hodin s retencí záloh po dobu 30 dnů.

### 4.3 Uživatelské rozhraní
* **NF3.1: Responzivita** – Návrh rozhraní metodou Mobile-First pro plnou optimalizaci na mobilních zařízeních.
* **NF3.2: Vizuální režimy** – Nativní podpora tmavého režimu (Dark Mode) jako výchozího zobrazení aplikace.

### 4.4 Bezpečnost
* **NF4.1: Krypto-ochrana hesel** – Striktní zákaz ukládání hesel v plain-textu. Povinná implementace jednosměrného hashování s využitím silného algoritmu (Argon2id nebo bcrypt s dynamickým saltem).
* **NF4.2: Zabezpečený kanál** – Šifrování veškerého síťového provozu pomocí protokolu HTTPS (TLS v1.3).
* **NF4.3: Sanitační vrstva** – Ochrana proti injektáži kódu (SQL Injection, XSS) pomocí striktní validace vstupních dat na backendu.

### 4.5 Technologická omezení
* **NF5.1: Aplikační vrstva (Backend)** – Node.js prostředí s architekturou postavenou na TypeScriptu (vhodné pro zachování typové bezpečnosti).
* **NF5.2: Datová vrstva** – Relační databázový systém PostgreSQL zajišťující referenční integritu složitých relací mezi uživateli, hardwarem a modifikacemi.
