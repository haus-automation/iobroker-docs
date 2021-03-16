.. _basics-architecture:

Architektur
===========

Der ioBroker ist modular aufgebaut und besteht aus mehreren Komponenten. In der Mitte finden wir das Herzstück des Systems: Den `js-controller`.

Dieser Controller verwaltet alles im Systemen. Von dort werden die einzelnen Instanzen gestartet und mit diesen kommuniziert.

Adapter
-------

Ein Adapter ist ein Stück Software, welches eine bestimmte Aufgabe übernimmt. Diese Aufgabe kann alles mögliche sein.

Es gibt verschiedenste Adapter, welche zum Beispiel

- mit Hardware kommunizieren
- Daten aus dem Internet abrufen
- Logiken bereitstellen
- Schnittstellen in den ioBroker von außen öffnen
- verschiedene Visualisierungsoberflächen bereitstellen
- den Sonnenstand berechnen
- Feiertage für dein Bundesland ermitteln
- die Administrationsoberfläche bereitstellen
- uvm.

Die Liste der verfügbaren Adapter ist extrem lang und umfasst über 250 verschiedene Integrationen.

Am Ende ist diese Liste dein Baukasten. Was Du brauchst, installierst Du dazu. Was Du nicht brauchst, lässt Du weg. Ganz einfach.

Instanzen
---------

Eine Instanz ist am Ende der eigentliche Prozess, welcher für einen Adapter gestartet wird. Für jeden Adapter können beliebig viele Prozesse (Instanzen) gestartet werden.

Nehmen wir mal das Beispiel Textverarbeitung. Microsoft Word ist also der Adapter und wird einfach nur einmalig installiert. Du kannst aber dann beliebig viele Word-Dokumente parallel öffnen, ohne mehrfach Word installieren zu müssen. Diese Prozesse sind dann die Instanzen.

Warum sollte man mehrere Instanzen für Adapter erstellen? In der Regel kommt das nicht vor. Aber angenommen Du hast mehrere Hue-Bridges von Philips. Dann würde jede Instanz die Kommunikation mit einer Hue-Bridge übernehmen. Du bräuchtest also genauso viele Instanzen des Hue-Adapters wie Du Hue-Bridges zu Hause hast.

