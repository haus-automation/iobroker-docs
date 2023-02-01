.. _development-adapter:

Adapter-Entwicklung
===================

Auf dem Weg zum eigenen/neuen Adapter gibt es eine Menge Werkzeuge, welche das Entwickler-Leben leichter machen und sehr schnell erste Ergebnisse ermöglichen!

Eine Sammlung von Tools, Links und Informationen ist im `ioBroker Dev-Portal <https://www.iobroker.dev>`_ zu finden (mit GitHub-Account einloggen!).

Um einen Überblick zu bekommen, wie die Adapter-Entwicklung ablaufen kann, habe ich folgenden Video-Leitfaden (zusätzlich zu dieser Seite) für Dich erstellt:

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto; margin-bottom: 2em;">
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/A9UETXyAmL4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

Voraussetzungen
---------------

1. Ein Linux-System, auf welchem noch kein ioBroker installiert wurde (z.B. Debian)
2. Ein Konto auf GitHub und npmjs
3. Grundlegende Kenntnisse in JavaScript (und optional TypeScript)
4. Erfahrung mit dem ioBroker und der Arbeitsweise (Objekte, Zustände, Datentypen, Rollen, ...)

Neuer Adapter
-------------

**Bevor Du einen neuen Adapter entwickelst, prüfe auf GitHub, ob dieser schon existiert oder angefangen wurde.**

Offene Anfragen für Adapter sind `hier <https://github.com/ioBroker/AdapterRequests/issues>`_ zu finden.

Wenn eine neuer Adapter entwickelt werden soll, empfiehlt es sich die Erstellung des Grundgerüstes mit Hilfe des Create-Adapter-Tools durchzuführen. Dieses Tool
erstellt auf Basis eines Templates ein komplett neues Projekt, mit welchem man direkt durchstarten kann.

`Create-Adapter-Tool (GitHub) <https://github.com/ioBroker/create-adapter>`_.

Um einen neuen Adapter zu erstellen, führe folgenden Befehl aus:

.. code:: console

    npx @iobroker/create-adapter

Wenn das Programm ausgeführt wird, werden einige Fragen gestellt, wie man gerne arbeiten möchte. Das Ergebnis sieht beispielsweise so aus:

.. code:: console

    Need to install the following packages:
        @iobroker/create-adapter
    Ok to proceed? (y) y

    =====================================================
    Welcome to the ioBroker adapter creator v2.1.1!
    =====================================================

    You can cancel at any point by pressing Ctrl+C.

    Let's get started with a few questions about your project!
    ✔ Please enter the name of your project: · gira-iot
    ✔ Which title should be shown in the admin UI? · Gira IoT
    ✔ Please enter a short description: · Integrate your Gira X1 or HomeServer
    ✔ Enter some keywords (separated by commas) to describe your project: · gira,x1,homeserver,iot
    ✔ If you have any contributors, please enter their names (seperated by commas): · 

    Nice! Let's get technical...
    ✔ How detailed do you want to configure your project? · yes
    ✔ Which features should your project contain? · adapter
    ✔ Which additional features should be available in the admin? · No items were selected
    ✔ Which category does your adapter fall into? · iot-systems
    ✔ When should the adapter be started? · daemon
    ✔ From where will the adapter get its data? · local
    ✔ How will the adapter receive its data? · push
    ✔ Do you want to indicate the connection state? · yes

    Some more questions about the source code...
    ✔ Which language do you want to use to code the adapter? · JavaScript
    ✔ Use React for the Admin UI? · no
    ✔ Which of the following tools do you want to use? · ESLint, type checking
    ✔ Would you like to automate new releases with one simple command? · yes
    ✔ Would you like to use dev-server to develop and test your code with a simple command line tool? · yes
    ✔ Please choose the port number on which dev-server should present the admin web interface: · 8081
    ✔ Do you prefer tab or space indentation? · Space (4)
    ✔ Do you prefer double or single quotes? · single
    ✔ How should the main adapter file be structured? · yes

    Almost done! Just a few administrative details...
    ✔ Please enter your name (or nickname): · Matthias Kleine
    ✔ What's your name/org on GitHub? · klein0r
    ✔ What's your email address? · info@haus-automatisierung.com
    ✔ Which protocol should be used for the repo URL? · SSH
    ✔ Initialize the GitHub repo automatically? · yes
    ✔ How should your default Git branch be called? · main
    ✔ Which license should be used for your project? · MIT License
    ✔ Do you want to receive regular dependency updates through Pull Requests? · yes

Danach werden automatisch alle nötigen Dateien erstellt und es kann direkt mit der Entwicklung gestartet werden!

*Natürlich könnte man auch alle Dateien manuell anlegen - aber das ist nicht zu empfehlen und bedeutet sehr viel mehr Arbeit!*

Dev-Server
----------

Um das neue Projekt lokal auszuführen, kann der sog. Dev-Server verwendet werden. Dieser legt in einem Unterverzeichnis des aktuellen Projektes eine ioBroker-Installation an und kann diese bei bedarf starten. So hat man für jeden Adapter eine eigene Entwicklungsumgebung. Das schöne ist: JavaScript-Dateien werden automatisch überwacht und ein Neustart der Adapter-Instanz durchgeführt, sobald sich etwas ändert.

`Zum ioBroker Dev-Server <https://github.com/ioBroker/dev-server>`_

GitHub Repository
-----------------

.. tip::
    Es ist empfehlenswert, den Quellcode für den Adapter auf GitHub zu veröffentlichen. Natürlich würden andere Plattformen wie Amazon Code Commit oder Bitbucket genauso funktionieren, allerdings arbeitet der Großteil der ioBroker-Community mit GitHub. Und darauf ist das komplett ioBroker-Universrum ausgelegt.

Wichtig ist, dass man den Namenskonventionen für ein neues Repository folgt (darum kümmert sich der Adapter-Creator ebenfalls). Das **Repository** heißt dann ``ioBroker.<deinadapter>``. **Auf Groß- und Kleinschreibung achten!** Das B von ioBroker wird im Repository-Namen groß geschrieben! Der Rest wird klein geschrieben!

Gültige Namen für ein neues **Repository** wären also zum Beispiel:

- ``ioBroker.admin``
- ``ioBroker.javascript``
- ``ioBroker.luftdaten``
- ``ioBroker.octoprint``

.. warning::
    Es ist darauf zu achten, dass der gewählte Name für einen Adapter noch nicht vergeben ist! Die oben genannten Beispiele sind alle schon vorhanden. Ansonsten kann der neue Adapter später nicht veröffentlicht werden bzw. in die Adapter-Liste mit aufgenommen werden.

Beschäftige Dich also auf jeden Fall mit diesen Themen:

- ``git commit``
- ``git push``
- ``git remote`` / Remote Repositories
- Branches undTags
- SSH Key Authentication / SSH Key Agent / SSH Key Forwarding

Übersetzungen
-------------

Generell ist es sinnvoll, einen neuen Adapter (direkt von Anfang an) in mehrere Sprachen zu übersetzen. Die "Basis-Sprache" ist dabei Englisch. Von dort wird in andere Sprachen übersetzt.

.. note::
    Generell gab es schon viele Ansätze und Werkzeuge, welche Dir bei Übersetzungen im ioBroker helfen sollten. Angefangen von Webseiten, bis zu irgendwelchen gulp-Scripts. Vieles davon existiert heute noch in den meisten Adaptern.

Der aktuellste Weg ist das Paket - `Adapter-Dev <https://github.com/ioBroker/adapter-dev>`_ (``npm i --save-dev @iobroker/adapter-dev``). Anstatt also Dateien in zig unterschiedlichen Versionen hin und her zu kopieren, sollte dieses Paket verwendet werden!

Alternativ, gibt es vom ioBroker-Team ein Tool, welches einen Englischen Text in alle andere Sprachen übersetzt und im richtigen Format für den ioBroker zurückliefert (JSON).

`Zum ioBroker Translator <https://translator.iobroker.in>`_

Gibst man zum Beispiel ``today`` ein, liefert das Programm die folgenden Übersetzungen im JSON-Format:

.. code:: json

    {
        "today": {
            "en": "today",
            "de": "heute",
            "ru": "сегодня",
            "pt": "hoje",
            "nl": "vandaag",
            "fr": "aujourd'hui",
            "it": "oggi",
            "es": "hoy dia",
            "pl": "dzisiaj",
            "uk": "сьогодні",
            "zh-cn": "今天"
        }
    }

Diese Informationen können direkt im Adapter verwendet werden.

**Es ist darauf zu achten, dass ALLE Texte übersetzt sind (inklusive Objekt-Namen).**

.. note::
    Leider ist es so, dass (wie üblich) die erstellen Übersetzungen nicht immer einwandfrei sind. Häufig ist z.B. die Deutsche Übersetzung falsch oder ergibt keinen Sinn. Eine manuelle Kontrolle ist in jedem Fall sinnvoll. Ist die Übersetzung von Englisch zu Deutsch korrekt, dann passt es höchstwahrscheinlich auch in den anderen Sprachen.

Alle Texte **müssen** in die folgenden Sprachen übersetzt werden:

- Englisch (en)
- Deutsch (de)

Alle Text **sollten** zusätzlich auch diese Sprachen übersetzt werden:

- Russisch (ru)
- Portugisisch (pt)
- Niederländisch (nl)
- Französisch (fr)
- Italienisch (it)
- Spanisch (es)
- Polnisch (pl)
- Ukrainisch (uk)
- Chinesisch (zh-cn)

npm
---

Sobald es einen "Release" des neuen Adapters gibt, wird eine Versionsnummer vergeben. Dabei ist auf `semantische Versionierung <https://semver.org/lang/de/>`_ zu achten!

Die erste Version des Adapters wird also höchstwahrscheinlich die ``0.0.1`` sein.

Generell werden nodejs-Pakete über ``npm`` veröffentlicht. Dieser Paketmanager kümmert sich um deine Abhängigkeiten im Projekt (package.json) und von dort werden auch die Pakete bei der Installation des Adapters geladen.

.. tip::
    Es gibt im Adapter-Creator-Tool (siehe oben) verschiedene Scripts, welche Dir automatisch beim Erstellen eines neuen Releases das Paket auf npmjs.com veröffentlichen. Dafür musst Du ein Token erstellen, welches im GitHub-Repository hinterlegt wird.

Folgende Themen sind wichtig (Schlüsselwörter für Google):

- semantische Versionierung
- `npmjs.com <https://docs.npmjs.com>`_
- ``package.json``
- ``npm install``
- publish von neuen npm Paketen

.. note::
    Generell haben GitHub und npmjs erstmal nichts miteinadner zu tun. Das sind zwei unterschiedliche Plattformen. GitHub hilft bei der Entwicklung und Issue-Tracking, während npm das fertige Pakete vorhält und an die Nutzer ausliefert. Über diverse Integrationsmöglichen greifen diese beiden Plattformen aber ineinander und vereinfachen den Workflow!

**Der Name des Paketes für npm unterscheidet sich dabei vom Namen des Repository!** Hier wird das "B" in ioBroker nicht mehr groß geschrieben! Der Paket-Name enthält also nur Kleinbuchstaben.

Gültige Namen für ein neues **npm Paket** wären also beispielsweise:

- ``iobroker.admin``
- ``iobroker.javascript``
- ``iobroker.luftdaten``
- ``iobroker.octoprint``

*Sollte der Adapter mit dem oben genannten Tool erstellt worden sein, wird dies bereits automatisch berücksichtigt!*

Adapter prüfen
--------------

Für einen Adapter gibt es eine Liste an Regeln, welche die Qualität der Adapter erhöhen sollen. Entspricht ein Adapter nicht diesen Anforderungen, wird er nicht in die (offizielle) Liste der verfügbaren Adapter aufgenommen!

Diese Regeln einzuhalten ist relativ einfach, da der ``ioBroker Adapter Checker`` genau sagt, was noch getan werden muss bzw. falsch läuft.

Sobald also eine erste Version von einem Adapter fertig ist, alles ins GitHub-Repository gepusht wurde und ein Paket auf npmjs.com veröffentlich wurde, kann der Adapter-Checker gestartet werden:

`Zum ioBroker Adapter-Checker <https://adapter-check.iobroker.in/>`_

**Dort wird die URL von einem GitHub-Repository eingefügt.**

Wichtig ist, dass alle Haken grün sind und möglichst keine Warnungen ausgegeben werden.

.. tip::
    Es ist sinnvoll, schon während der Entwicklung regelmäßig zu prüfen, ob ein Adapter den Anforderungen entspricht.

Generell gilt, dass auch hier die Entwicklung weiter geht. Es werden regelmäßig mehr Prüfungen hinzugefügt oder andere entfernt. Wenn ein Adapter also heute alle Tests besteht, muss das bei der nächsten Version nicht mehr unbedingt so sein. Der Repository-Checker wird in unregelmäßigen Abständen Issues in deinem Repository anlegen, falls etwas nicht stimmen sollte.

Das `Repository <https://github.com/ioBroker/ioBroker.repochecker>`_ vom Adapter-Checker kann mit neuen Regeln erweitert werden (siehe ``index.js``).

Adapter veröffentlichen
-----------------------

Soll der neue Adapter nun auch anderen zur Verfügung gestellt werden, sollte dieser von erfahrenen Nutzerb im ioBroker Forum getestet werden. Dazu kann ein neuer `Foren-Beitrag <https://forum.iobroker.net/category/91/tester>`_ mit der Bitte um einen Test erstellt werden.

**Danach** kann ein Pull-Request im `GitHub Repository (ioBroker.repositories) <https://github.com/ioBroker/ioBroker.repositories>`_ erstellt werden, mit welchem der neue Adapter dort hinzugefügt wird. Mehr Details hier: :ref:`ecosystem-repositories`.

.. note::
    Adapter können abgelehnt werden, wenn nicht alle Adapter-Checks (siehe oben) erfüllt sind.

Links
-----

- `ioBroker Dev-Portal <https://www.iobroker.dev>`_
- `Create-Adapter <https://github.com/ioBroker/create-adapter>`_
- `Adapter-Dev <https://github.com/ioBroker/adapter-dev>`_
- `Adapter-Checker <https://adapter-check.iobroker.in/>`_
- `Release-Script von AlCalzone <https://github.com/AlCalzone/release-script>`_
- `Adapter-Examples <https://github.com/ioBroker/ioBroker.example>`_
