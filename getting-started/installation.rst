.. _getting-started-installation:

Installation
============

Generell lässt sich der ioBroker auf jedem Betriebssystem installieren, auf welchem auch `nodejs <https://nodejs.org/en/>`_ läuft.

Fertige Images
--------------

In der Vergangenheit wurden z.B. für den Raspberry Pi fertige Images angeboten, welche aber heute nicht mehr verwendet werden sollten.

Manuelle Installation (Linux)
-----------------------------

.. note::
    In älteren Anleitungen liest man häufig, dass noch weitere Pakete (wie nodejs) vorher auf dem System installiert werden müssen. Das ist aber nicht mehr nötig, da sich der ioBroker-Installer selbst um die Abhängigkeiten kümmert und diese installiert!

Unter Linux lässt sich der ioBroker mit nur einem einzigen Befehl installieren:

``curl -sLf https://iobroker.net/install.sh | bash -``

Dieser Befehl bereitet das komplette System vor und richtet alles nötige ein:

- nodejs wird heruntergeladen und installiert
- Neuer ioBroker-Benutzer wird erstellt
- ioBroker wird an die richtige Stelle im System installiert
- Es werden die wichtigsten Adapter installiert und Instanzen hinzugefügt (Admin, Discovery, ...)
- Ein Autostart für ioBroker wird eingerichtet

Danach kann über den Standard-Port 8083 im Browser die Admin-Oberfläche aufgerufen werden.