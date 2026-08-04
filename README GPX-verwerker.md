# GPX-Verwerker
Automatisch hernoemen en sorteren van GPX-routebestanden op basis van locatie (land, provincie, plaats) via de OpenStreetMap Nominatim API.

---

## Wat doet het?

Het programma verwerkt alle `.gpx` bestanden in een door jou gekozen map en doet het volgende:

- Bepaalt automatisch het **land**, de **provincie** en de **plaats** van het startpunt via OpenStreetMap
- **Hernoemt** elk bestand naar een gestandaardiseerde naam, bijvoorbeeld:
  `ned-ov-mijnroute_raalte_12.3_km.gpx`
- **Sorteert** de bestanden in mappen: `output_landen → Land → Provincie → Plaats`
- Voegt een correct **waypoint** en **trackmetadata** toe aan elk GPX-bestand
- Exporteert een **CSV-overzicht** (`gpx_output.csv`) van alle verwerkte bestanden

---

## Oorsprong: van `gpx.py` naar `gpx_gui.py`

Het originele script `gpx.py` was een commandline-programma: je startte het vanuit een terminal, het verwerkte alle GPX-bestanden in dezelfde map als het script zelf, en de uitvoer verscheen als tekst in het terminalvenster.

`gpx_gui.py` is de grafische versie daarvan. De volledige verwerkingslogica (hernoemen, locatie opzoeken, mappen aanmaken, CSV schrijven) is **ongewijzigd overgenomen** uit `gpx.py`. Wat er is toegevoegd en veranderd:

| Welk onderdeel | Origineel `(gpx.py)`               | Grafisch `(gpx_gui.py)`             |
|:-------------- |:---------------------------------- |:----------------------------------- |
| Mapkeuze       | Altijd de map van het script zelf  | Vrij te kiezen via bestandskiezer   |
| Voortgang      | `print()` in terminal              | Logvenster + voortgangsbalk in GUI  |
| Starten        | `python gpx.py` in terminal        | Dubbelklikken                       |
| Blokkering     | Verwerking blokkeert terminal      | Verwerking op achtergrondthread     |
| Afsluiten      | Automatisch na afloop              | Venster blijft open                 |

Het originele `gpx.py` blijft gewoon bruikbaar als je de voorkeur geeft aan de commandline.

---

## Bestanden

| Gebruikte bestanden   | Omschrijving                                      |
|:----------------------|:--------------------------------------------------|
| `gpx.py`              | Het originele commandline-script                  |
| `gpx_gui.py`          | De Python broncode met grafische interface        |
| `bouwen.bat`          | Bouwt een zelfstandige `.exe` (Windows)           |
| `installeer.bat`      | Installeert benodigde Python-pakketten (Windows)  |
| `GPX_Verwerker.bat`   | Start het programma via Python (Windows)          |
| `installeer.sh`       | Installeert benodigde pakketten (Linux)           |
| `gpx_verwerker.sh`    | Start het programma via Python (Linux)            |
| `README.md`           | Deze handleiding                                  |

---

## Gebruik

### Optie 1 - Zelfstandige EXE (aanbevolen, Windows)

Geen Python nodig op de doelcomputer.

1. Zorg dat Python is geïnstalleerd op jouw pc (eenmalig, alleen voor het bouwen)
   → [python.org/downloads](https://www.python.org/downloads/) — vink **"Add Python to PATH"** aan
2. Zet `bouwen.bat` en `gpx_gui.py` in dezelfde map
3. Dubbelklik op **`bouwen.bat`**
4. Er opent een venster om je `.ico` icoontje te kiezen (optioneel)
5. Na ~1 minuut staat `GPX_Verwerker.exe` klaar - dit is jouw programma

> **Let op:** Windows Defender kan een waarschuwing geven bij de eerste start ("onbekende uitgever").
> Klik op **Meer info → Toch uitvoeren**.

---

### Optie 2 - Via Python (Windows)

Python moet geïnstalleerd zijn op de computer waarop je het programma gebruikt.

1. Dubbelklik **eenmalig** op `installeer.bat` om `requests` te installeren
2. Daarna: dubbelklik altijd op **`GPX_Verwerker.bat`** om het programma te starten

---

### Optie 3 - Via Python (Linux)

1. Open een terminal in de map met de bestanden
2. Voer **eenmalig** uit:
   ```bash
   bash installeer.sh
   ```
3. Daarna: dubbelklik op **`gpx_verwerker.sh`** in je bestandsbeheerder,
   of voer uit via terminal:
   ```bash
   ./gpx_verwerker.sh
   ```

> **Let op:** `tkinter` zit niet altijd standaard in Python op Linux.
> Installeer het indien nodig:
> ```bash
> # Ubuntu/Debian
> sudo apt install python3-tk
>
> # Fedora
> sudo dnf install python3-tkinter
>
> # Arch
> sudo pacman -S tk
> ```

---

### Optie 4 - Commandline (origineel)

Wil je zonder GUI werken, zoals het originele script?

1. Zet `gpx.py` in de map met je GPX-bestanden
2. Open een terminal in die map en voer uit:
   ```bash
   python gpx.py
   ```
3. De uitvoer verschijnt direct in het terminalvenster

---

## Het programma gebruiken

1. Klik op **Bladeren…** en selecteer de map met jouw GPX-bestanden
2. Het programma telt automatisch hoeveel bestanden er gevonden zijn
3. Klik op **▶ Start verwerking**
4. Volg de voortgang in het logvenster:
   - ✅ Groen = succesvol verwerkt
   - ⚠️ Oranje = waarschuwing (bijv. geen trackpunten)
   - ❌ Rood = fout bij verwerking
5. Na afloop vind je de resultaten in de submap **`output_landen`**

---

## Mapstructuur uitvoer

```
output_landen/
├── Nederland/
│   └── Overijssel/
│       └── Raalte/
│           └── ned-ov-mijnroute_raalte_12.3_km.gpx
├── Duitsland/
│   └── Nordrhein-Westfalen/
│       └── ...
gpx_output.csv
```

---

## Naamgeving bestanden

De nieuwe bestandsnaam volgt altijd dit formaat:

```
{landcode}-{provinciecode}-{originele naam}_{plaats}_{afstand}_km.gpx
```

Voorbeeld: `ned-ov-zondagsrit_raalte_23.7_km.gpx`

---

## Ondersteunde landen

Nederland, Duitsland, België, Luxemburg, Oostenrijk, Italië, Frankrijk, Zwitserland, Liechtenstein, Denemarken, Zweden, Noorwegen.

---

## Vereisten (alleen bij gebruik via Python)

- Python 3.8 of hoger
- Pakket: `requests` (wordt automatisch geïnstalleerd via `installeer.bat` / `installeer.sh`)
- `tkinter` (standaard aanwezig in Python op Windows; op Linux apart installeren — zie boven)

---

## Internetverbinding

Het programma heeft een **actieve internetverbinding** nodig tijdens de verwerking. De locatiegegevens worden per bestand opgezocht via de gratis [Nominatim API](https://nominatim.openstreetmap.org/) van OpenStreetMap. Uit respect voor deze gratis dienst wordt er automatisch een kleine pauze gehouden tussen opzoekacties (max. 1 per seconde).
