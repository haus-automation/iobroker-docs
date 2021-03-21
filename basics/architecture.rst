.. _basics-architecture:

Architektur
===========

Der ioBroker ist modular aufgebaut und besteht aus mehreren Komponenten. In der Mitte finden wir das Herzstück des Systems: Den ``js-controller``.

Dieser Controller verwaltet alles im System. Von dort werden die einzelnen Instanzen gestartet und mit diesen kommuniziert.

Adapter
-------

Ein Adapter ist ein Stück Software, welches eine bestimmte Aufgabe übernimmt. Diese Aufgabe kann alles mögliche sein.

Es gibt daher verschiedenste Adapter, welche zum Beispiel

- mit Hardware kommunizieren (Philips Hue, KNX, HomeMatic, Loxone, ...)
- Daten aus dem Internet abrufen (Wetter, Verkehr, ...)
- Logiken bereitstellen (Szenen, Regeln, ...)
- Schnittstellen in den ioBroker von außen öffnen (Zugriff von unterwegs)
- verschiedene Visualisierungsoberflächen bereitstellen (Steuerung per Tablet)
- den Sonnenstand berechnen
- Feiertage für dein Bundesland ermitteln
- die Administrationsoberfläche bereitstellen (admin)
- Backups erstellen
- Verbindungen zu Datenbanken herstellen
- uvm.

Die Liste der verfügbaren Adapter ist extrem lang und umfasst über 250 verschiedene Integrationen.

Diese Liste ist Dein Baukasten. Was Du brauchst, installierst Du dazu. Was Du nicht brauchst, lässt Du weg. Ganz einfach.

.. note::
    Nach der Installation eines Adapters wird automatisch eine neue Instanz erstellt, um Dir das Leben leichter zu machen.

Damit Du einfacher den richtigen Adapter für eine Aufgabe finden kannst, sind Adapter in Kategorien eingeteilt:

- Allgemein
- Aufbewahrung
- Visualisierung
- Energie
- Logik
- ...

Instanzen
---------

Eine Instanz ist am Ende der eigentliche Prozess, welcher für einen Adapter gestartet wird. Für jeden Adapter können beliebig viele Prozesse (Instanzen) gestartet werden.

Nehmen wir mal das Beispiel Textverarbeitung. *Microsoft Word* ist also der Adapter und wird einfach nur einmalig installiert. Du kannst aber dann beliebig viele Word-Dokumente parallel öffnen, ohne das Programm Word mehrfach installieren zu müssen. Diese Prozesse sind dann die Instanzen.

Warum sollte man mehrere Instanzen für Adapter erstellen? In der Regel kommt das nicht vor. Aber angenommen Du hast mehrere Hue-Bridges von Philips im Haus. Dann würde jede Instanz die Kommunikation mit genau einer Hue-Bridge übernehmen. Du bräuchtest also genauso viele Instanzen des Hue-Adapters wie Du Hue-Bridges zu Hause hast.

.. image:: /images/ioBrokerDoku-Adapter.png
    :alt: js-controller Adapter

Ein weiteres Beispiel wären die Wetterdaten von verschiedenen Orten. Möchtest Du bei Dir zu Hause die Regenwahrscheinlichkeit ermitteln, installierst Du einen Wetter-Adapter und konfigurierst diesen auf deinen Wohnort. Wenn Du dann noch ein Ferienhaus hast, erstellst Du eine weitere Instanz, welche auf den entsprechenden Ort konfiguriert wird und somit von dort die Daten liefert.

Da es also **mehrere Instanzen vom gleichen Adapter** geben kann, werden diese Instanzen durchnummeriert. Standardmäßig startet die Numerierung bei Null. In diesem Beispiel also:

- ``hue.0``
- ``hue.1``
- ``hue.2``

Von den meisten Adaptern wirst Du aber nur eine einzelne Instanz haben, welche die Nummer Null (0) bekommt.

.. note::
    Während Adapter "nur" die Software bereitstellen, enthalten Instanzen die spezifische Konfiguration. Der Adapter gibt dabei nur vor, WAS konfiguriert werden kann. Die exakte Konfiguration wird in der jeweiligen Instanz gespeichert (wie zum Beispiel der Wohnort im Wetter-Adapter).

Jede Instanz wird dabei (normalerweise) in einem eigenen Prozess gestartet. Das hat zur Folge, dass der Prozess das restliche System nicht beeinflussen kann. Sollte also ein Adpater abstürzen oder nicht mehr laufen, funktioniert der Rest trotzdem weiter! Dieses Vorgehen braucht zwar deutlich mehr Ressourcen als bei vergleichbaren Lösungen, aber sorgt für viel mehr Stabilität.

Selbst die Admin-Oberfläche ist eine Instanz eines Adapters, welche genau wie alle anderen Instanzen mit dem ``js-controller`` spricht. Theoretisch könnte man also den ioBroker auch komplett ohne den Admin-Adapter betreiben. Dieser bietet einfach nur eine möglichst komfortable Oberfläche um dein System zu verwalten.

js-controller
-------------

Kommen wir noch einmal zurück zum angesprochenen ``js-controller``. Dieser startet die Prozesse der einzelnen Instanzen und verwaltet die Kommunikation zu diesen.

Bleiben wir bei dem Beispiel von Philips Hue. Nachdem also eine neue Instanz für den installierten Adapter gestartet wurde, legt der Adapter Objekte an, welche Deine Räume, Szenen und Lampen repräsentieren, welche Deine Philips Hue Bridge kennt. Diese Objekte kann er aber nicht selbst anlegen, sondern nur den ``js-controller`` darum bitten dies zu tun. Also sendet der Hue-Adapter eine Nachricht an diesen Prozess, und übermittelt die Informationen für das Anlegen der Objekte.

Genauso können sich Instanzen beim ``js-controller`` registrieren, dass diese bestimmte Informationen abrufen möchten. Angenommen die Visualisierung möchte immer den aktuellen Status von Philips Hue Lampen auf einer Webseite darstellen. In dem Fall würde der Adapter der Visualisrung den ``js-controller`` bitten, bei jeder Änderung eines Status im Philips Hue Adapter informiert zu werden. So wird vermieden, dass jede Instanz über jede Änderung im System informiert wird. Ansonsten würden ohne Ende irrelevante Nachrichten durch das System gesendet. Und das vermeidet dieses Konzept.

.. note::
    Falls Du einige Begriffe hier noch nicht verstanden haben solltest, werden diese in den anderen Grundlagen-Dokumentation noch im Detail erklärt!

.. image:: /images/ioBrokerDoku-Instanzen.png
    :alt: js-controller Instanzen
