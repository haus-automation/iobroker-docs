.. _getting-started-installation:

Installation
============

Generell lässt sich der ioBroker auf jedem Betriebssystem installieren, welchs auch `nodejs <https://nodejs.org/en/>`_ unterstützt.

Fertige Images
--------------

In der Vergangenheit wurden z.B. für den Raspberry Pi fertige Images angeboten, welche aber heute nicht mehr verwendet werden sollten! Eine manuelle Installation ist immer zu bevorzugen.

Manuelle Installation (Linux)
-----------------------------

.. note::
    In älteren Anleitungen liest man häufig, dass noch weitere Pakete (wie nodejs) vorher auf dem System installiert werden müssen. Das ist aber nicht mehr nötig, da sich der ioBroker-Installer selbst um die Abhängigkeiten kümmert und diese installiert!

Unter Linux (z.B. Debian oder Ubuntu) lässt sich der ioBroker mit nur einem einzigen Befehl installieren:

.. code:: console

    curl -sLf https://iobroker.net/install.sh | bash -

Dieser Befehl bereitet das komplette System vor und richtet alles nötige ein:

- ``nodejs`` wird in der aktuell empfohlenen Version heruntergeladen und installiert
- Ein neuer System-Benutzer wird erstellt (ioBroker)
- Der ioBroker wird an die richtige Stelle im System installiert
- Es werden die wichtigsten Adapter installiert und Instanzen hinzugefügt (Admin, Discovery, ...)
- Ein Autostart für ioBroker wird eingerichtet
- ...

Danach kann über den Standard-Port 8081 im Browser Deiner Wahl die Admin-Oberfläche aufgerufen werden.

Manuelle Installation (Windows)
-------------------------------

Natürlich kann der ioBroker auch unter Windows installiert werden. Ich persönliche sehe Windows aber als Desktop-Betriebsystem, welches nicht als Server verwendet werden sollte. Eine Installation unter Linux ist daher immer zu bevorzugen.

Docker-Image
------------

Das offizielle Docker-Image (von buanet) findest Du hier: `Official Docker Image for ioBroker <https://github.com/buanet/ioBroker.docker>`_

Auf den GitHub-Seiten ist außerdem erklärt, wie man mit dem Image arbeitet.
