# Energy-Forecasting-Anomaly-Detection-Basel
Wir entwickeln für Basel-Stadt ein datengetriebenes System zur Energie-Lastprognose und Anomalie-Erkennung. Auf Basis historischer Last-, Wetter- und Kalenderdaten sollen kurz- bis mittelfristige Forecasts gelingen und Abweichungen früh präventiv erkannt werden um Beschaffung, Netzbetrieb und Peak-Management verlässlicher und kosteneffizienter zu machen.


## Methodik
### CRISP-DM
Im Rahmen unseres ML-Projektes haben wir uns für die CRISP-DM-Zyklus-Vorgehensweise (Cross Industry Standard Process for Data Mining) entschieden. Dieses Vorgehensmodell gliedert den gesamten Datenanalyseprozess, von der Problemdefinition bis zur praktischen Nutzung des Modells, in sechs klar definierte Phasen.
CRISP-DM bietet uns eine strukturierte und iterative Vorgehensweise, um systematisch Daten zu verstehen, Modelle zu entwickeln und deren Nutzen im realen Umfeld zu evaluieren.
<img width="547" height="501" alt="image" src="https://github.com/user-attachments/assets/bd337948-9179-4135-807f-55f50c3d952d" />


### Business Understanding Data Understanding (Problemformalisierung)
- In dieser Phase wird das Problem fachlich verstanden und in eine formalisierte Aufgabenstellung für das Machine Learning übersetzt.
Zudem erfolgt eine erste Analyse der vorhandenen Datenquellen und -qualität.
### Data Preperation
- Die Daten werden aufbereitet, bereinigt und für das Modelltraining vorbereitet (Handling fehlender Werte,, Diskretisierung, Skalierung)
### Modeling
- Auswahl und Training des Modells (versch. Kriterien, Stärken & Schwächen und Parameter kennen)
### Evaluation
- Überprüfung der Modellleistung anhand geeigneter Metriken.
- Ziel ist es, die Ergebnisse kritisch zu bewerten, Probleme zu erkennen und den wirtschaftlichen Nutzen zu beurteilen.
### Deployment
- das entwickelte Modell oder die gewonnenen Erkenntnisse in die Praxis überführen.

(Das trainierte Prognosemodell wird als wiederverwendbare Python-Datei gespeichert. Ein separates Skript ruft das Modell regelmäßig auf und erstellt täglich eine Vorhersage des Stromverbrauchs für den Folgetag. Die Prognosen werden automatisch in einer CSV-Datei oder als Diagramm gespeichert.
Dadurch ist eine einfache, reproduzierbare Nutzung der Ergebnisse möglich, ohne manuelle Eingriffe.
### SCRUM

## Business Understanding
### Ziel und Scope
Das Projekt entwickelt ein zweiteiliges System für Basel-Stadt:
- Nachfrageprognose des Stromverbrauchs auf 15-Minuten-Basis.
- Anomalieerkennung im Verbrauchsverhalten.

Datenumfang: viertelstündlicher Netzbezug im Kanton Basel-Stadt inklusive Netzverluste.
Nicht enthalten: lokal erzeugter und direkt vor Ort verbrauchter PV-Strom.
Kundengruppen im Datensatz: grundversorgte und freie Kunden. Beide beeinflussen die Netzlast.

Zielvariable: Gesamtverbrauch (Summe beider Kundengruppen).
Für Netzführung und Lastplanung ist der gesamte Effekt im Netz relevant. Optional können später Submodelle pro Kundengruppe erstellt werden.

### Hintergrund
IWB Industrielle Werke Basel ist Versorger und Netzbetreiber: eigene Erzeugung (Wasserkraft, KWK, Solar) und marktseitiger Zukauf bei Bedarf.

Grundversorgte Kunden: beziehen Energie direkt von IWB.
Freie Kunden: kaufen Energie am Markt, nutzen aber das IWB-Netz. Deren Last ist für die Netzplanung ebenso relevant.

### Business Nutzten
#### Nachfrageprognose
1. Planungssicherheit: Eigenproduktion, Speicher, Lastverschiebung und gezielter Marktzukauf können früh geplant werden.
2. Kosteneffizienz: Weniger teurer Spot-Zukauf, optimierte Fahrpläne für Erzeugung und Speicher, geringere Regelenergie.

3. Operative Steuerung: Personaleinsatz, Instandhaltung und Anlagenbetrieb können an den erwarteten Verbrauch angepasst werden.

Hinweis: IWB kauft nicht täglich Strom, sondern nur bei Bedarf. Prognosen dienen dazu, diese Entscheidungen präziser und kostengünstiger zu treffen.

#### Anomalieerkennung
- Erkannt werden ungewöhnliche Verbrauchsmuster, keine technischen Netzstörungen.

- Vorgehen: Vergleich Ist-Verbrauch vs. erwarteter Verbrauch (Prognosefehler oder statistische Abweichung).

- Keine Netzkapazitätsdaten nötig – Fokus liegt auf Verbrauchsanomalien (sprunghafte Peaks, untypische Tagesmuster, fehlerhafte Messungen).

- Nutzen: Frühwarnung für Prüfungen, Qualitätssicherung, Messfehler-Erkennung und Auffälligkeiten im Verbrauchsverhalten.

### Technischer Zielzustand
Für das Projekt wird ein reproduzierbares Feature-Set entwickelt, das zeitliche Merkmale (z. B. Stunde, Wochentag, Monat, saisonale Muster) sowie abgeleitete Werte wie Lags, gleitende Durchschnitte und optional Feiertagsinformationen umfasst. Auf dieser Basis wird ein Regressionsmodell trainiert, um den Stromverbrauch präzise vorherzusagen und die Modellgüte anhand transparenter Metriken zu bewerten. Zusätzlich wird ein Anomalie-Flag pro Zeitintervall erzeugt, das auf Prognoseabweichungen oder unüberwachten Scores basiert. Die täglichen Prognosen und Erkennungen werden automatisch in einer CSV-Datei oder als Diagramm exportiert.

## Data Understnading Main

### Datenquellen:
#### Stromverbauch Daten von Basel
- [Data](data/251006_StromverbrauchBasel2012-2025.csv)
- [Link](https://de.wikipedia.org/wiki/Data_Science](https://opendata.swiss/de/dataset/kantonaler-stromverbrauch-netzlast))
#### Meteo Daten
- [Data]()
- [Link]()

## 📊 Data Understanding – Stromverbauch Daten von Basel (2012–2025)

### 🗂️ Dataset Overview
- **File:** `251006_StromverbrauchBasel2012-2025.csv`  
- **Records:** 481,959 rows  
- **Columns:** 12  
- **Time Range:** 2012-01-01 → 2025-09-29  
- **Frequency:** 15-minute intervals  
- **Index:** `Start der Messung` (UTC, DatetimeIndex)

---

### 🧩 Data Structure
| Type | Columns |
|------|----------|
| **Numeric** | `Stromverbrauch`, `Grundversorgte Kunden`, `Freie Kunden`, `Jahr`, `Monat`, `Tag`, `Wochentag`, `Tag des Jahres`, `Quartal`, `Woche des Jahres` |
| **Text** | `Start der Messung (Text)` |

---

### 🧠 Data Quality
- ✅ **No duplicate timestamps**  
- ⚠️ **Missing values:**
  - `Grundversorgte Kunden`: ~62 % missing  
  - `Freie Kunden`: ~63 % missing  
- ✅ **Time intervals** consistent (every 15 min)  
- ✅ **DatetimeIndex** correctly set (`UTC`)  

---

### 📈 Descriptive Statistics
| Variable | Min | Max | Mean |
|-----------|-----|-----|------|
| Stromverbrauch (kWh) | 22 322 | 68 374 | **38 454** |
| Grundversorgte Kunden | 0 | 26 090 | **15 788** |
| Freie Kunden | 0 | 32 296 | **19 277** |

---

### 🔗 Correlations (top relations)
- `Stromverbrauch` ↔ `Freie Kunden`: **0.92**  
- `Stromverbrauch` ↔ `Grundversorgte Kunden`: **0.87**  
- `Stromverbrauch` ↔ `Wochentag`: **–0.27**

➡️ Indicates strong dependency between consumption and customer activity.

---

### 🔍 Key Insights
1. Dataset covers over **13 years** of Basel’s power consumption.  
2. **Slight weekend and yearly seasonality** visible.  
3. Customer segmentation (free vs. base supply) strongly affects load patterns.  
4. Ideal for **time-series forecasting and ML model training**.  
5. Minor missing data, but overall **high data quality**.

---

### 🚀 Next Steps
- Handle missing customer data (interpolation / imputation).  
- Perform seasonal decomposition (trend + seasonality).  
- Integrate weather and solar data for advanced forecasting.  
- Prepare training dataset for **short- and mid-term energy load prediction**.

---

*Generated automatically using Python (pandas + matplotlib) in the `data_analysis.py` pipeline.*

## Data Preparation

## Modeling
## Evaluation
## Demployment
