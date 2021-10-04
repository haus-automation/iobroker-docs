.. _ecosystem-repositories:

Repositories
============

Welche Adapter zur Verfügung stehen, wird in sogenannten Repositories hinterlegt. Die Liste an verfügbaren Repositories kann man selbst ändern, um zum Beispiel ein externes Repository zu nutzen. In den allermeisten Fällen wird dies aber niemand machen, sondern nur die Standard-Repositories nutzen.

Generell gibt es zwei verschiedene Adapter-Listen (Repositories), welche vom ioBroker-Team angeboten werden:

- ``stable`` - wird täglich aktualisiert und hier bereitgestellt: ``http://download.iobroker.net/sources-dist.json``
- ``beta`` (früher auch ``latest`` genannt) - wird täglich aktualisiert und hier bereitgestellt: ``http://download.iobroker.net/sources-dist-latest.json``

Pflege der Listen
-----------------

Beide Listen werden in `diesem GitHub Repository (ioBroker.repositories) <https://github.com/ioBroker/ioBroker.repositories>`_ gepflegt.

- ``stable`` = ``sources-dist-stable.json``
- ``beta`` bzw. ``latest`` = ``sources-dist.json``

Im ``stable`` werden getestete Adapter aufgenommen. Dort wird neben dem Repository auch eine genaue Version mit angegeben.
Ein Eintrag sieht dort zum Beispiel so aus:

.. code:: json

    "admin": {
        "meta": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/io-package.json",
        "icon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
        "type": "general",
        "version": "4.2.1"
    }

Wie Du siehst, ist vom Admin-Adapter in diesem Beispiel aktuell die Version ``4.2.1`` als stabil definiert.

Es kann gut sein, dass auf npm mittlerweile neue Versionen vergeben wurden und diese auch veröffentlicht ist. An diese Version kommt man, wenn man als Verwahrungsort das ``beta`` Repository wählt.

Im Gegensatz dazu hat der Eintrag im ``beta`` keine definierte Versionsnummer:

.. code:: json

    "admin": {
        "meta": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/io-package.json",
        "icon": "https://raw.githubusercontent.com/ioBroker/ioBroker.admin/master/admin/admin.png",
        "type": "general"
    }

Bei dem ``beta`` Repository wird automatisch immer die letzte freigegebene Version zum Update angeboten (von npm).

Dieses Vorgehen hat den Vorteil, dass man als Adapter-Entwickler genau steuern kann, welche Nutzer welche Version angeboten bekommen. So können neue Versionen zwar veröffentlicht werden, aber "stable-Nutzer" werden erst später auf eine neue Version gebracht, wenn diese von vielen "beta-Nutzern" bereits getestet wurden.

.. note::
    Es kann vorkommen, dass einige Adapter zwar im beta-Repository vorhanden sind, aber noch nicht im stable-Repository zu finden sind (weil noch in Entwicklung bzw. noch keine stabile Version verfügbar)!

.. image:: /images/ioBrokerDoku-Repositories.png
    :alt: ioBroker-Repositories

Einstellungen im ioBroker
-------------------------

Der ioBroker kann zwar mehrere Repositories verwalten (zum Beispiel über den Admin-Adapter), aber nur ein einzelnes Repository kann aktiv sein.

Das aktive Repository wird dabei im Objekt ``system.config`` im Attribut ``common.activeRepo`` hinterlegt. Siehe :ref:`basics-systemconfig`.

Update-Prozess
--------------

Das konfigurierte/aktive Repository wird regelmäßig geprüft. Dafür wird die jeweils angegebene URL geändert, sodass stattdessen eine Hash-Datei abgerufen wird.

.. code:: javascript

    urlOrPath = urlOrPath.replace(/\.json$/, '-hash.json');

So wird also z.B. statt ``http://download.iobroker.net/sources-dist.json`` erstmal ``http://download.iobroker.net/sources-dist-hash.json`` abgerufen. Aktuell hat die Datei folgenden Inhalt:

.. code:: json

    {
        "hash": "a3276c4275647354fa9f81748dde7941",
        "date": "2021-10-04T14:21:02.483Z",
        "name": "sources-dist.json"
    }

Dieser Hash wird mit dem aktuellen Hash in ``system.repositories`` verglichen. Sollte der Hash abweichen, wird die eigentliche JSON-Datei geladen. Dies wurde so gelöst, um den Traffic von tausenden anfragenden Systemen zu reduzieren.
