````markdown
# Chasing Your Tail (CYT)

Komplexný analyzátor Wi-Fi probe requestov, ktorý monitoruje a sleduje bezdrôtové zariadenia analýzou ich probe požiadaviek. Systém sa integruje s Kismet na zachytávanie paketov a WiGLE API na geolokačnú analýzu SSID, pričom obsahuje pokročilé schopnosti detekcie sledovania.

## 🚨 Bezpečnostné upozornenie

Projekt bol bezpečnostne zosilnený na odstránenie kritických zraniteľností:
- **Prevencia SQL injekcií** pomocou parametrizovaných dotazov  
- **Šifrovaná správa poverení** pre API kľúče  
- **Validácia vstupov** a sanitizácia  
- **Bezpečné načítavanie ignore listu** (už žiadne volania `exec()`)

**⚠️ POVINNÉ: Spustite `python3 migrate_credentials.py` pred prvým použitím na zabezpečenie API kľúčov!**

## Funkcie

- **Reálne monitorovanie Wi-Fi** s integráciou Kismet  
- **Pokročilá detekcia sledovania** so scoringom perzistencie  
- **🆕 Automatická GPS integrácia** – získavanie súradníc z Bluetooth GPS cez Kismet  
- **Korelácia GPS** a zoskupovanie polohy (100 m prah)  
- **Profesionálna KML vizualizácia** pre Google Earth s interaktívnym obsahom  
- **Viacformátové reporty** – Markdown, HTML (pandoc), KML  
- **Časové okná sledovania** (5, 10, 15, 20 minút)  
- **WiGLE API integrácia** na geolokáciu SSID  
- **Algoritmy sledovania viacerých lokalít** pre detekciu nasledovania  
- **Rozšírené GUI** s tlačidlom na analýzu sledovania  
- **Organizovaná štruktúra výstupov**  
- **Kompletné logovanie a analytické nástroje**

## Požiadavky

- Python 3.6+  
- Kismet na zachytávanie bezdrôtových paketov  
- Wi-Fi adaptér s podporou monitor módu  
- Linuxový systém  
- WiGLE API kľúč (voliteľné)  

## Inštalácia a nastavenie

### 1. Inštalácia závislostí
```bash
pip3 install -r requirements.txt
````

### 2. Bezpečnostné nastavenie (POVINNÉ PRI PRVOM SPOUSTENÍ)

```bash
# Migrácia poverení z nezabezpečeného config.json
python3 migrate_credentials.py

# Overenie zabezpečenia
python3 chasing_your_tail.py
# Výstup: "🔒 SECURE MODE: All SQL injection vulnerabilities have been eliminated!"
```

### 3. Konfigurácia systému

Upravte `config.json` s cestami a nastaveniami:

* Cesta ku Kismet databáze
* Log a ignore list adresáre
* Časové okná
* Geografické hranice vyhľadávania

## Použitie

### GUI rozhranie

```bash
python3 cyt_gui.py
```

**Funkcie GUI:**

* 🗺️ **Analýza sledovania** – GPS-korelácia s KML vizualizáciou
* 📈 **Analýza logov** – historické probe requesty
* Monitoring v reálnom čase s notifikáciami

### Monitorovanie z príkazovej riadky

```bash
python3 chasing_your_tail.py
./start_kismet_clean.sh
```

### Analýza dát

```bash
python3 probe_analyzer.py
python3 probe_analyzer.py --days 7
python3 probe_analyzer.py --all-logs
python3 probe_analyzer.py --wigle
```

### Detekcia sledovania a vizualizácia

```bash
python3 surveillance_analyzer.py
python3 surveillance_analyzer.py --demo
python3 surveillance_analyzer.py --kismet-db /path/to/kismet.db
python3 surveillance_analyzer.py --stalking-only --min-persistence 0.8
python3 surveillance_analyzer.py --output-json analysis_results.json
python3 surveillance_analyzer.py --gps-file gps_coordinates.json
```

### Správa ignore listov

```bash
python3 legacy/create_ignore_list.py
```

**Poznámka:** Ignore listy sú teraz uložené v `./ignore_lists/`

## Hlavné komponenty

* **chasing\_your\_tail.py**: jadro monitorovania
* **cyt\_gui.py**: GUI s analýzou sledovania
* **surveillance\_analyzer.py**: GPS detekcia sledovania a KML vizualizácia
* **surveillance\_detector.py**: engine na detekciu perzistencie
* **gps\_tracker.py**: sledovanie GPS s KML generovaním
* **probe\_analyzer.py**: post-processing s WiGLE integráciou
* **start\_kismet\_clean.sh**: funkčný Kismet startup skript

### Bezpečnostné komponenty

* **secure\_database.py**: ochrana proti SQL injekcii
* **secure\_credentials.py**: šifrovaná správa kľúčov
* **secure\_ignore\_loader.py**: bezpečné načítanie ignore listov
* **secure\_main\_logic.py**: bezpečná logika monitorovania
* **input\_validation.py**: validácia vstupov
* **migrate\_credentials.py**: migrácia poverení

## Výstupné súbory a štruktúra

* **Surveillance Reports**: `./surveillance_reports/*.md`, `*.html`
* **KML Vizualizácie**: `./kml_files/*.kml`
* **CYT Logy**: `./logs/`
* **Analysis Logs**: `./analysis_logs/`
* **Probe Reports**: `./reports/`

### Konfigurácia a dáta

* **Ignore Lists**: `./ignore_lists/*.json`
* **Poverenia**: `./secure_credentials/encrypted_credentials.json`

### Archívy

* **old\_scripts/**: staré skripty
* **docs\_archive/**: poznámky a zálohy
* **legacy/**: pôvodný kód pred zabezpečením

## Architektúra

### Časové okná

4 prekrývajúce sa okná: 5, 10, 15, 20 minút

### Detekcia sledovania

* **Časová perzistencia**
* **Korelácia polohy**
* **Analýza SSID vzorov**
* **Časová analýza**
* **Skórovanie perzistencie**
* **Viaclokalizované sledovanie**

### GPS integrácia a KML

* Automatické GPS z Kismet
* Zoskupovanie polohy (100 m)
* KML vizualizácia s farebnými značkami, trasami, heatmapami

## Konfigurácia

Všetko v `config.json`:

```json
{
  "kismet_db_path": "/path/to/kismet/*.kismet",
  "log_directory": "./logs/",
  "ignore_lists_directory": "./ignore_lists/",
  "time_windows": {
    "recent": 5,
    "medium": 10,
    "old": 15,
    "oldest": 20
  }
}
```

## Bezpečnostné vlastnosti

* Parametrizované SQL dotazy
* Šifrované uloženie kľúčov
* Validácia vstupov
* Audit logovanie
* Bezpečné ignore listy

## Autor

@matt0177

## Licencia

MIT License

## Upozornenie

Nástroj je určený na legitímny bezpečnostný výskum, správu sietí a osobnú bezpečnosť. Použitie musí byť v súlade so zákonmi.

```
```
