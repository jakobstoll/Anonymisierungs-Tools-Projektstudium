# Datenaufbereitung für die Anonymisierung medizinischer Daten

Dieses Repository enthält spezialisierte Werkzeuge zur Transformation synthetischer Patientendaten. Die Tools wurden im Rahmen des Projektstudiums **"Technische Ansätze zur Anonymisierung medizinischer Daten: Im Spannungsfeld zwischen Datenschutz und Datenverfügbarkeit"** an der OTH Regensburg selbstständig entwickelt.

## Hintergrund und Zielsetzung
Die von der Software *Synthea* generierten Daten liegen in einer relationalen CSV-Struktur vor. Für eine Risikoanalyse und Anonymisierung mit dem Tool *ARX* ist jedoch ein flacher Datensatz (ein Eintrag pro Subjekt) zwingend erforderlich. Die hier bereitgestellten Werkzeuge automatisieren diesen mehrstufigen ETL-Prozess (Extract, Transform, Load), um einen konsistenten Analysedatensatz von 10.000 Patienten aus Massachusetts zu erstellen.

## Übersicht der Werkzeuge

| Werkzeug | Methode | Primäres Ziel |
| :--- | :--- | :--- |
| **fix_zip.html** | API-basierte Korrektur | Reparatur invalider Postleitzahlen (ZIP-Codes) |
| **transform.html** | Daten-Aggregation | Überführung in ein ARX-kompatibles Flat-Format |

### 1. ZIP-Code Reparatur (fix_zip.html)
Synthea-Exporte weisen häufig unvollständige Adressdaten auf, bei denen Postleitzahlen als "00000" hinterlegt sind.
* **Funktionsweise**: Das Tool identifiziert diese Lücken in der Datei `patients.csv`.
* **Korrektur**: Über die *Zippopotam.us-API* werden die korrekten Postleitzahlen anhand der hinterlegten Stadtnamen (Region Massachusetts) automatisiert ergänzt.
* **Integrität**: Die restliche Datenstruktur des Datensatzes bleibt dabei vollständig erhalten.

### 2. Datentransformation (transform.html)
Dieses Werkzeug dient der Aggregation relationaler Diagnosen zur Erfüllung der ARX-Anforderungen.
* **Daten-Aggregation**: Das Tool identifiziert alle Einträge in einem (bereits verknüpften) Datensatz, die zur selben Patienten-ID (UUID) gehören.
* **Flattening**: Da nach der Verknüpfung von Patienten- und Krankheitsdaten pro Diagnose eine separate Zeile existiert, fasst dieses Werkzeug alle Diagnosen eines Patienten kommagetrennt in einer einzigen Zelle zusammen.
* **Datenbasis**: Das Ergebnis ist eine ARX-konforme Datei mit genau einer Zeile pro Individuum, was die notwendige Voraussetzung für die korrekte Berechnung der k-Anonymität erfüllt.

## Technische Spezifikationen
* **Plattform**: Clientseitige HTML5/JavaScript-Anwendungen.
* **Anforderungen**: Keine lokale Installation erforderlich; die Ausführung erfolgt direkt im Webbrowser.
* **Bibliotheken**: Verwendung von *PapaParse* für eine effiziente CSV-Verarbeitung im Browser.

## Live-Anwendung
Die Werkzeuge können direkt im Browser genutzt werden, ohne dass ein Download oder eine lokale Installation erforderlich ist:

* [Tool zur ZIP-Code Reparatur öffnen](https://jakobstoll.github.io/Anonymisierungs-Tools-Projektstudium/fix_zip.html)
* [Tool zur Datentransformation (Flattening) öffnen](https://jakobstoll.github.io/Anonymisierungs-Tools-Projektstudium/transform.html)

## Nutzungshinweise
1. **Schritt 1**: Öffnen Sie `fix_zip.html` und laden Sie die ursprüngliche `patients.csv` hoch, um die Adressdaten zu korrigieren.
2. **Schritt 2**: Verknüpfen Sie die korrigierte Datei (z. B. via SVERWEIS in Excel) mit der `conditions.csv`.
3. **Schritt 3**: Öffnen Sie `transform.html` und laden Sie diesen verknüpften Datensatz hoch.
4. **Schritt 4**: Wählen Sie die Spalten für die Gruppierung (ID) und die Aggregation (Diagnose) aus und laden Sie die finale, ARX-konforme Datei herunter.

---

**Autor:** Jakob Stoll  
**Akademischer Kontext:** Projektstudium Wirtschaftsinformatik, OTH Regensburg  
**Datum:** Dezember 2025
