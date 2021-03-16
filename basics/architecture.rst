.. _basics-architecture:

Architektur
===========

Der ioBroker ist modular aufgebaut und besteht aus mehreren Komponenten. In der Mitte finden wir das Herzstück des Systems: Den `js-controller`.

Dieser Controller verwaltet alles im Systemen. Von dort werden die einzelnen Instanzen gestartet und mit diesen kommuniziert.

Adapter
-------

Ein Adapter ist ein Stück Software, welches eine bestimmte Aufgabe übernimmt. Diese Aufgabe kann alles mögliche sein.

Es gibt verschiedenste Adapter, welche zum Beispiel

- mit Hardware kommunizieren (Philips Hue, KNX, HomeMatic, Loxone, ...)
- Daten aus dem Internet abrufen (Wetter, Verkehr, ...)
- Logiken bereitstellen
- Schnittstellen in den ioBroker von außen öffnen
- verschiedene Visualisierungsoberflächen bereitstellen
- den Sonnenstand berechnen
- Feiertage für dein Bundesland ermitteln
- die Administrationsoberfläche bereitstellen
- Backups erstellen
- Verbindungen zu Datenbanken herstellen
- uvm.

Die Liste der verfügbaren Adapter ist extrem lang und umfasst über 250 verschiedene Integrationen.

Am Ende ist diese Liste dein Baukasten. Was Du brauchst, installierst Du dazu. Was Du nicht brauchst, lässt Du weg. Ganz einfach.

Instanzen
---------

Eine Instanz ist am Ende der eigentliche Prozess, welcher für einen Adapter gestartet wird. Für jeden Adapter können beliebig viele Prozesse (Instanzen) gestartet werden.

Nehmen wir mal das Beispiel Textverarbeitung. Microsoft Word ist also der Adapter und wird einfach nur einmalig installiert. Du kannst aber dann beliebig viele Word-Dokumente parallel öffnen, ohne mehrfach Word installieren zu müssen. Diese Prozesse sind dann die Instanzen.

Warum sollte man mehrere Instanzen für Adapter erstellen? In der Regel kommt das nicht vor. Aber angenommen Du hast mehrere Hue-Bridges von Philips. Dann würde jede Instanz die Kommunikation mit einer Hue-Bridge übernehmen. Du bräuchtest also genauso viele Instanzen des Hue-Adapters wie Du Hue-Bridges zu Hause hast.

js-controller
-------------

Kommen wir noch einmal zurück zum angesprochenen `js-controller`. Dieser startet die Prozesse der einzelnen Instanzen und verwaltet die Kommunikation zu diesen.

Bleiben wir bei dem Beispiel von Philips Hue. Nachdem also eine neue Instanz für den installierten Adapter gestartet wurde, legt der Adapter Objekte an, welche Deine Räume, Szenen und Lampen repräsentieren, welche Deine Philips Hue Bridge kennt. Diese Objekte kann er aber nicht selbst anlegen, sondern nur den `js-controller` darum bitten dies zu tun. Also sendet der Hue-Adapter eine Nachricht an diesen Prozess, und übermittelt die Informationen für das Anlegen der Objekte.

Genauso können sich Instanzen beim `js-controller` registrieren, dass diese bestimmte Informationen abrufen möchten. Angenommen die Visualisierung möchte immer den aktuellen Status von Philips Hue Lampen auf einer Webseite darstellen. In dem Fall würde der Adapter der Visualisrung den `js-controller` bitten, bei jeder Änderung eines Status im Philips Hue Adapter informiert zu werden. So wird vermieden, dass jede Instanz über jede Änderung im System informiert wird. Ansonsten würden ohne Ende irrelevante Nachrichten durch das System gesendet. Und das vermeidet dieses Konzept.

.. note::
    Falls Du einige Begriffe hier noch nicht verstanden haben solltest, werden diese in den anderen Grundlagen-Dokumentation noch im Detail erklärt!

.. image:: /images/ioBrokerDoku-Instanzen.png
    :alt: js-controller Instanzen