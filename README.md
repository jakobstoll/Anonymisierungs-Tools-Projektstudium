# Datenaufbereitung für die Anonymisierung medizinischer Daten

Dieses Repository enthält spezialisierte Werkzeuge zur Transformation synthetischer Patientendaten. Die Tools wurden im Rahmen des Projektstudiums **"Technische Ansätze zur Anonymisierung medizinischer Daten: Im Spannungsfeld zwischen Datenschutz und Datenverfügbarkeit"** selbstständig entwickelt.

## Hintergrund und Zielsetzung
Die von der Software *Synthea* generierten Daten liegen in einer relationalen CSV-Struktur vor. Für eine Risikoanalyse und Anonymisierung mit dem Tool *ARX* ist jedoch ein flacher Datensatz (ein Eintrag pro Subjekt) zwingend erforderlich. Die hier bereitgestellten Werkzeuge automatisieren diesen mehrstufigen ETL-Prozess (Extract, Transform, Load), um einen konsistenten Analysedatensatz von 10.000 Patienten aus Massachusetts zu erstellen.

## Übersicht der Werkzeuge

| Werkzeug | Methode | Primäres Ziel |
| :--- | :--- | :--- |
| **fix_zip.html** | API-basierte Korrektur | Reparatur invalider Postleitzahlen (ZIP-Codes) |
| **transform.html** | Daten-Aggregation | Überführung in ein ARX-kompatibles Flat-Format |

### 1. ZIP-Code Reparatur (fix_zip.html)
Synthea-Exporte weisen häufig unvollständige Adressdaten auf, bei denen Postleitzahlen als "00000" hinterlegt sind.
* **Funktionsweise**: Das Tool identifiziert diese Lücken in der `patients.csv`.
* **Korrektur**: Über die *Zippopotam.us-API* werden die korrekten Postleitzahlen anhand der hinterlegten Stadtnamen (Massachusetts) automatisiert ergänzt.
* **Integrität**: Die restliche Datenstruktur des Datensatzes bleibt dabei vollständig erhalten.

### 2. Datentransformation (transform.html)
Dieses Werkzeug dient der Aggregation relationaler Diagnosen zur Erfüllung der ARX-Anforderungen.
* **UUID-Mapping**: Die Stammdaten werden über die eindeutige Patienten-ID (UUID) mit den Einträgen der `conditions.csv` verknüpft.
* **Flattening**: Da Synthea für jede Diagnose eine separate Zeile generiert, aggregiert dieses Tool alle Diagnosen eines Patienten in einer einzigen Zeile.
* **Datenbasis**: Das Ergebnis ist eine ARX-konforme Datei, die genau einen Datensatz pro Individuum enthält.

## Technische Spezifikationen
* **Plattform**: Clientseitige HTML5/JavaScript-Anwendungen.
* **Anforderungen**: Keine lokale Installation erforderlich; die Ausführung erfolgt direkt im Webbrowser.
* **Bibliotheken**: Verwendung von *PapaParse* für eine effiziente CSV-Verarbeitung im Browser.

## Live-Anwendung
Die Werkzeuge können direkt im Browser genutzt werden, ohne dass ein Download oder eine lokale Installation erforderlich ist:

* [Tool zur ZIP-Code Reparatur öffnen](https://jakobstoll.github.io/Anonymisierungs-Tools-Projektstudium/fix_zip.html)
* [Tool zur Datentransformation (Flattening) öffnen](https://jakobstoll.github.io/Anonymisierungs-Tools-Projektstudium/transform.html)

## Nutzungshinweise
1. Öffnen Sie das gewünschte Tool über die oben stehenden Links.
2. Laden Sie die entsprechende Synthea-CSV-Datei hoch.
3. Prüfen Sie die Spaltenzuordnung (eine automatische Erkennung ist integriert).
4. Starten Sie den Prozess und laden Sie die transformierte Datei für den Import in ARX herunter.

---

**Autor:** Jakob Stoll  
**Akademischer Kontext:** Projektstudium Wirtschaftsinformatik, OTH Regensburg  
**Datum:** Dezember 2025
