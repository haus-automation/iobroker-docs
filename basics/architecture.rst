.. _basics-architecture:

Architektur
===========

Der ioBroker ist modular aufgebaut und besteht aus mehreren Komponenten. In der Mitte steht das Herzstück des Systems: Der ``js-controller``.

Dieser Controller verwaltet alles im System. Von dort werden die einzelnen Instanzen gestartet und mit diesen kommuniziert.

Adapter
-------

Ein Adapter ist ein Stück Software, welches eine bestimmte Aufgabe übernimmt. Diese Aufgabe kann alles mögliche sein.

Es gibt daher verschiedenste Adapter, welche zum Beispiel

- mit Hardware kommunizieren (Philips Hue, KNX, HomeMatic, Loxone, ...)
- Daten aus dem Internet abrufen (Wetter, Verkehr, ...)
- Logiken bereitstellen (Szenen, Regeln, ...)
- Schnittstellen in den ioBroker von außen öffnen / Cloud-Verbindungen (Zugriff von unterwegs)
- verschiedene Visualisierungsoberflächen bereitstellen (Steuerung per Tablet)
- den Sonnenstand berechnen
- Feiertage für dein Bundesland ermitteln
- die Administrationsoberfläche bereitstellen (admin)
- Backups erstellen
- Verbindungen zu Datenbanken herstellen
- uvm.

Die `Liste der verfügbaren Adapter <https://www.iobroker.net/#de/adapters>`_ ist extrem lang und umfasst mittlerweile über 400 verschiedene Integrationen.

Diese Liste ist ein Baukasten. Was man braucht wird dazu installiert. Was nicht gebraucht wird, wird nicht installiert. Ganz einfach. Bei einer neuen ioBroker-Installation wird nur eine Hand voll Adapter mitgeliefert. Der Rest muss manuell installiert und konfiguriert werden.

Damit der richtige Adapter für eine Aufgabe einfacher gefunden werden kann, sind Adapter in verschiedene Kategorien eingeteilt. Unter anderem:

- Allgemein
- Aufbewahrung
- Visualisierung
- Energie
- Logik
- ...

Instanzen
---------

Eine Instanz ist am Ende der eigentliche **Prozess**, welcher für einen Adapter gestartet wird. Für (fast) jeden Adapter können beliebig viele Prozesse (Instanzen) gestartet werden.

Ein Beispiel: Es soll auf einem PC ein Brief geschrieben werden. *Microsoft Word* ist dabei also der Adapter und wird einfach nur einmalig installiert. Danach können aber beliebig viele Word-Dokumente parallel geöffnet werden, ohne das Programm *Word* mehrfach installiert werden muss. Diese Prozesse sind dann die Instanzen. Was genau diese Dateien darstellen, hängt von der geöffneten *Word*-Datei ab - diese Datei wäre dann im ioBroker-Umfeld die Instanz-Konfiguration.

.. note::
    Nach der Installation eines Adapters wird automatisch eine neue Instanz erstellt, daher werden die Begriffe Adapter und Instanz von vielen Nutzern häufig durcheinander geworfen. Man installiert Adapter und konfiguriert / startet / stoppt die einzelnen Instanzen.

**Warum sollte man mehrere Instanzen von einem Adapter erstellen?** In der Praxis kommt das relativ selten vor. Aber angenommen es gibt mehrere Hue-Bridges von *Philips* im Haus. Dann würde jede Instanz die Kommunikation mit genau einer Hue-Bridge übernehmen. Man bräuchte also genau so viele Instanzen des Hue-Adapters wie Hue-Bridges vorhanden sind.

.. image:: /images/ioBrokerDoku-Adapter.png
    :alt: js-controller Adapter

Ein weiteres Beispiel wären die Wetterdaten von verschiedenen Orten. Möchte man an seinem Wohnort die Regenwahrscheinlichkeit ermitteln, wird ein Wetter-Adapter installiert und eine Instanz mit dem Heimatort konfiguriert. Soll dann aber noch das Wetter beim Ferienhaus integriert werden, wird eine weitere Instanz erstellt, welche auf den entsprechenden Ferienort konfiguriert wird (und somit von dort die Daten liefert).

Da es also **mehrere Instanzen vom gleichen Adapter** geben kann, werden diese Instanzen durchnummeriert. Standardmäßig startet die Numerierung bei ``0`` (numerisch Null). In diesem Beispiel also:

- ``hue.0``
- ``hue.1``
- ``hue.2``

Von den meisten Adaptern wird man aber nur eine einzelne Instanz erstellen, welche die Nummer ``0`` bekommt. Nur in seltenen Fällen werden mehrere Instanzen von einem Adapter erstellt. *Manche Adapter verbieten sogar das erstellen von mehreren Instanzen, weil es aus technischer Sicht keinen Sinn ergibt, mehrere Instanzen dieses Adapters zu haben. Das kommt aber sehr selten vor.*

.. note::
    Während Adapter "nur" die Software bereitstellen, enthalten Instanzen die spezifische Konfiguration. Der Adapter gibt dabei nur vor, WAS konfiguriert werden kann. Die exakte Konfiguration wird in der jeweiligen Instanz gespeichert (wie zum Beispiel der Wohnort im Wetter-Adapter oder die IP der Hue-Bridge im Hue-Adapter).

    Einen Adapter zu installieren braucht also erstmal nur Ressourcen auf der Festplatte. Eine neue Instanz davon zu erstellen braucht dann weitere Ressourcen wie CPU-Zeit oder Arbeitsspeicher.

Jede Instanz wird dabei (normalerweise) in einem eigenen Prozess gestartet. Das hat den Vorteil, dass der Prozess das restliche System nicht so stark beeinflussen kann. Sollte also ein Adpater abstürzen oder wegen einem Fehler nicht mehr starten, funktionieren die anderen ioBroker Adapter-Instanzen trotzdem weiter! Dieses Vorgehen braucht zwar deutlich mehr Ressourcen als bei vergleichbaren Lösungen, aber sorgt für viel mehr Stabilität. Um Ressourcen zu sparen, kann (zu lasten der Stabilität) der :ref:`basics-compactmode` aktiviert werden.

Selbst die Admin-Oberfläche ist nur eine Instanz eines Adapters, welche (genau wie alle anderen Instanzen) mit dem ``js-controller`` spricht. Theoretisch könnte man den ioBroker auch komplett ohne den Admin-Adapter betreiben. Dieser bietet einfach nur eine möglichst komfortable Oberfläche um das System zu verwalten.

js-controller
-------------

Kommen wir noch einmal zurück zum angesprochenen ``js-controller``. Dieser startet die Prozesse der einzelnen Instanzen und verwaltet die Kommunikation mit diesen.

Bleiben wir bei dem Beispiel von *Philips Hue*. Nachdem also eine neue Instanz für den installierten Adapter gestartet wurde, legt die laufende Instanz des Adapters sogenannte "Objekte" an, welche die Räume, Szenen und Lampen repräsentieren, welche die *Philips Hue Bridge* kennt. Diese Objekte kann er aber nicht selbst anlegen, sondern nur den ``js-controller`` darum bitten dies zu tun. Also sendet der Hue-Adapter eine Nachricht an diesen Prozess, und übermittelt die Informationen zum Anlegen der Objekte.

Genauso können sich Adapter-Instanzen beim ``js-controller`` registrieren, dass diese bestimmte Informationen nutzen möchten. Angenommen die Visualisierung möchte immer den aktuellen Status von *Philips Hue* Lampen auf einer Webseite darstellen. In dem Fall würde der Adapter der Visualisrung den ``js-controller`` bitten, bei jeder Änderung eines Status im *Philips Hue* Adapter informiert zu werden. So wird vermieden, dass jede Instanz über jede Änderung im System informiert wird. Ansonsten würden ohne Ende irrelevante Nachrichten durch das System gesendet. Und das vermeidet dieses Konzept.

Was genau diese "Objekte" sind und wie der ``js-controller`` Daten speichert, wird im nächsten Abschnitt unter :ref:`basics-datastorage` erklärt.

.. note::
    Falls Du einige Begriffe hier noch nicht verstanden haben solltest, werden diese in den anderen Grundlagen-Dokumentation noch im Detail erklärt!

.. image:: /images/ioBrokerDoku-Instanzen.png
    :alt: js-controller Instanzen

Links
-----

- `GitHub-Repository vom js-controller <https://github.com/ioBroker/ioBroker.js-controller>`_
