# Grundlagen Betriebssysteme
## Kapitel 2 - Einführung
### Definition Betriebssystem
> Das Betriebssystem wird gebildet durch die **Programme** eines digitalen Rechensystems, die zusammen mit den **Eigenschaften der Rechenanlage** die grundlage der möglichen BEtriebsarten des digitalen Rechensystems bilden und insbesondere die **Ausführung von Programmen steuern und überwachen**

### Definition Eingebettetes System
> Der Begriff des eingebetteten Systems bezeichnet einen Rechner, welcher dezentral in einem technischen Kontext eingebunden ist. Dieser Rechner aht die Aufgabe, das eingebettete System zu steuern, zu regel, zu überwachen oder auch die Benutzerinteraktion zu ermöglichen und ist speziell für die vorliegende Aufgabe angepasst

### BS-Hauptaufgaben
* Veredeln der Hardware
* Steuerung und Kontrolle der Programmausführung
* Interprozesskommunikation
* Verwaltung der Betriebsmittel
	* Zentrale Resourcen (Prozessoren, Arbeitsspeicher)
	* Periphere Ressourcen (Kommunikationseinheiten, Speichereinheiten)
* Anbieten von Diensten und Schnittstellen
* Sicherheitsmechanismen
* Arbeitsmodi
	* Benutzermodus (Begrenzte Auswahl an Maschinenbefehlen, HW-ZUgriff nur über BS, kein Schreibzugriff auf Systemcode)
	* Systemmodus (Alle ausführbaren Maschinenbefehle, Vollzugriff auf die HW, alleiniger Zugriff auf Systemcode und Daten)
	* Realisierung im Prozessor

### Betriebsarten
* Stapelverarbeitung
* Dialogbetrieb
* Echtzeitbetrieb
* Zweck: General Purpose vs. Special Purpose
* Betriebsziele
	* Hohe Auslastung
	* Kurze Antwortzeiten
	* Geringer Energieverbrauch

### Historie
#### 1. Generation (1945 - 1955)
Arbeiten an leerer Rechenanlage

#### 2. Generation (1955 - 1965)
Stapelbetrieb ohne parallele Verarbeitung

#### 3. Generation (1965 - 1980)
Mehrprogrammbetrieb, Dateisystem, Dialogbetrieb

#### 4. Generation (ab 1980)
Personal Computing, graphische Benutzeroberflächen, Netzwerkunterstützung, Multimedia

#### 5. Generation (ab 2000)
Eingebettete Systeme, Chipkarten, Mobilität, Ubiquität

### Betriebssystem-Architektur
#### Monolithischer Ansatz
* BS als umfangreiche Menge von Funktionen
* Wechelwirkungen (Aufrufe) dazwischen
* Zusammenfassung in einem BS-Kern
* Betreten des Kerns durch Systemaufrufe
* BS-Kern: arbeitet im Systemmodus, hat hohe Ablaufpriorität und ist permanent im RAM

#### Mikrokern-Ansatz
* Geschichtete Systeme
* BS erfüllt nur Basismechanismen, mögl. viele Subsysteme ausgelagert
* Systemfunktionen als Serverprozesse im Benutzermodus
* Wenig Speicherbedarf (kleiner Footprint)
* 