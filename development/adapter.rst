.. _development-adapter:

Adapter-Entwicklung
===================

In diesem Abschnitt fasse ich für Dich zusammen, was es bei der Entwicklung von neuen Adaptern zu beachten gibt. Auf dem Weg zum eigenen/neuen Adapter gibt es jede Menge Werkzeuge, welche Dir das Leben leichter machen und Dich sehr schnell zum Ziel bringen werden!

Neuer Adapter
-------------

**Bevor Du einen neuen Adapter entwickelst, schau am besten nach, ob dieser schon existiert oder angefangen wurde.**

Offene Anfragen für Adapter findest Du `hier <https://github.com/ioBroker/AdapterRequests/issues>`_.

Wenn Du einen neuen Adapter entwickeln möchtest, empfiehlt es sich die Erstellung mit Hilfe des Create-Adapter-Tools durchzuführen. Dieses Tool
erstellt Dir auf Basis eines Templates ein neues Projekt, mit welchem Du direkt durchstarten kannst.

Das Tool findest Du auf `GitHub <https://github.com/ioBroker/create-adapter>`_.

Um einen neuen Adapter zu erstellen, führst Du folgenden Befehl aus:

.. code:: 

    npx @iobroker/create-adapter 

Nachdem das Programm ausgeführt wird, werden Dir einige Fragen gestellt, wie Du gerne arbeiten möchtest. Das Ergebnis sieht beispielsweise so aus:

.. code:: console

    npx: Installierte 147 in 31.103s

    =====================================================
    Welcome to the ioBroker adapter creator v1.32.0!
    =====================================================

    You can cancel at any point by pressing Ctrl+C.

    Let's get started with a few questions about your project!
    ✔ Please enter the name of your project: · documentation
    ✔ Which title should be shown in the admin UI? · Documentation
    ✔ Please enter a short description: · An example adapter for the ioBroker documentation
    ✔ Enter some keywords (separated by commas) to describe your project: · documentation,learn,development
    ✔ If you have any contributors, please enter their names (seperated by commas): · 

    Nice! Let's get technical...
    ✔ How detailed do you want to configure your project? · yes
    ✔ Which features should your project contain? · adapter
    ✔ Which additional features should be available in the admin? · No items were selected
    ✔ Which category does your adapter fall into? · storage
    ✔ When should the adapter be started? · daemon
    ✔ From where will the adapter get its data? · local
    ✔ How will the adapter receive its data? · push
    ✔ Do you want to indicate the connection state? · yes
    ✔ Which language do you want to use to code the adapter? · JavaScript
    ✔ Use React for the Admin UI? · yes
    ✔ Which of the following tools do you want to use? · ESLint, type checking
    ✔ Do you prefer tab or space indentation? · Space (4)
    ✔ Do you prefer double or single quotes? · single
    ✔ How should the main adapter file be structured? · yes

    Almost done! Just a few administrative details...
    ✔ Please enter your name (or nickname): · Matthias Kleine
    ✔ What's your name/org on GitHub? · klein0r
    ✔ What's your email address? · info@haus-automatisierung.com
    ✔ Which protocol should be used for the repo URL? · SSH
    ✔ Initialize the GitHub repo automatically? · yes
    ✔ Which license should be used for your project? · MIT License
    ✔ Which continuous integration service should be used? · gh-actions
    ✔ Do you want to receive regular dependency updates through Pull Requests? · yes

Danach werden automatisch alle nötigen Dateien erstellt und Du kannst direkt mit der Entwicklung starten!

*Natürlich könntest Du auch alle Dateien manuell anlegen - aber das ist nicht zu empfehlen und bedeutet viel mehr Arbeit!*

GitHub Repository
-----------------

.. tip::
    Ich würde generell empfehlen, den Quellcode für den Adapter auf GitHub zu veröffentlichen. Natürlich würden andere Plattformen wie Amazon Code Commit oder Bitbucket genauso funktionieren, allerdings arbeitet der Großteil der ioBroker-Community mit GitHub.

Wichtig ist, dass Du den Namenskonventionen für ein neues Repository folgst. Das Repository heißt dabei ``ioBroker.<deinadapter>``. **Achte auf Groß- und Kleinschreibung!** Das B von ioBroker wird im Repository-Namen groß geschrieben! Der komplette Rest wird klein geschrieben!

Gültige Namen für Dein neues **Repository** wären also zum Beispiel:

- ``ioBroker.admin``
- ``ioBroker.javascript``
- ``ioBroker.luftdaten``
- ``ioBroker.octoprint``

.. warning::
    Achte darauf, dass der von Dir gewählte Name für einen Adapter noch nicht vergeben ist! Die oben genannten Beispiele sind alle schon vorhanden. Ansonsten kannst Du deinen Adapter später nicht veröffentlichen / in die Adapter-Liste mit aufnehmen.

Beschäftige Dich also auf jeden Fall mit diesen Themen:

- ``git commit``
- ``git push``
- Remote repositories
- Branches
- Tags
- SSH Key Authentication

Übersetzungen
-------------

Generell ist es sinnvoll, direkt von Anfang an deinen neuen Adapter in mehrere Sprachen zu übersetzen. Die "Basis-Sprache" sollte Englisch sein. Von dort wird in andere Sprachen übersetzt.
Damit Du das nicht manuell machen musst, gibt es vom ioBroker-Team ein Tool, welches Dir einen Englischen Text in andere Sprachen übersetzt und im richtigen Format für den ioBroker zurückliefert.

`Zum ioBroker Translator <https://translator.iobroker.in>`_.

Gibst Du dort zum Beispiel ``today`` ein, liefert Dir das Programm folgende Übersetzungen im JSON-Format:

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
            "zh-cn": "今天"
        }
    }

Diese Informationen kannst Du direkt in deinem Adapter verwenden. Achte darauf, dass alle Texte übersetzt sind.

.. note::
    Natürlich ist es so, dass (wie üblich) die erstellen Übersetzungen nicht immer einwandfrei sind. Häufig ist z.B. die Deutsche Übersetzung einfachf falsch oder ergibt keinen Sinn. Kontrolliere noch einmal manuell, ob die Texte korrekt sind. Je mehr Sprachen, desto besser!

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
- Chinesisch (zh-cn)

npm
---

Sobald es einen Release deines Adapters gibt, solltest Du eine Versionsnummer vergeben. Achte dabei auf `semantische Versionierung <https://semver.org/lang/de/>`_!

Die erste Version deines Adapters wird also höchstwahrscheinlich die ``0.0.1`` sein.

Generell werden nodejs-Pakete über ``npm`` veröffentlicht. Dieser Paketmanager kümmert sich um deine Abhängigkeiten im Projekt und von dort werden auch die Pakete bei der Installation des Adapters geladen.

.. tip::
    Es gibt im Adapter-Creator-Tool (siehe oben) verschiedene Scripts, welche Dir automatisch beim Erstellen eines neuen Releases das Paket auf npm.js veröffentlichen. Dafür musst Du ein Token erstellen, welches im GitHub-Repository hinterlegt wird.

Beschäftige Dich also auf jeden Fall mit diesen Themen:

- semantische Versionierung
- `npmjs.org <https://docs.npmjs.com>`_
- ``package.json``
- publish von neuen npm Paketen

.. note::
    Generell haben GitHub und npmjs erstmal nichts miteinadner zu tun. Das sind zwei unterschiedliche Plattformen. GitHub hilft Dir bei der Entwicklung und Issue-Tracking, während npm das fertige Pakete vorhält und an die Nutzer ausliefert. Über diverse Integrationsmöglichen greifen diese beiden Plattformen aber ineinander und vereinfachen den Workflow.

**Der Name deines Paketes für npm unterscheidet sich dabei vom Namen des Repository!** Hier wird das "B" in ioBroker nicht mehr groß geschrieben! Der Paket-Name enthält also nur Kleinbuchstaben.

Gültige Namen für Dein neues **npm Paket** wären also zum Beispiel:

- ``iobroker.admin``
- ``iobroker.javascript``
- ``iobroker.luftdaten``
- ``iobroker.octoprint``

*Solltest Du den Adapter mit dem oben genannten Tool erstellt haben, wird dies bereits automatisch berücksichtigt!*

Adapter prüfen
--------------

Für einen Adapter gibt es eine Liste an Regeln, an welche Du Dich halten solltest. Entspricht Dein Adapter nicht diesen Anforderungen, wird er nicht in die offizielle Liste der verfügbaren Adapter aufgenommen!

Diese Regeln einzuhalten ist relativ einfach, da Dir der ``ioBroker Adapter Checker`` genau sagt, was noch getan werden muss bzw. falsch läuft.

Sobald Du also eine erste Version von deinem Adapter fertig hast, Du alles in GitHub-Repository gepusht hast und Dein Paket auf npmjs veröffentlich wurde, kannst Du den Adapter-Checker starten:

`Zum ioBroker Adapter-Checker <https://adapter-check.iobroker.in/>`_.

**Dort fügst Du die URL von deinem GitHub-Repository ein.**

Wichtig ist, dass alle Haken grün sind.

.. tip::
    Prüfe schon während der Entwicklung regelmäßig, ob dein Adapter den Anforderungen entspricht.

Das `Repository <https://github.com/ioBroker/ioBroker.repochecker>`_ vom Adapter-Checker kann mit neuen Regeln erweitert werden (siehe ``index.js``).

Adapter veröffentlichen
-----------------------

Möchtest Du deinen Adapter nun anderen zur Verfügung stellen, solltest Du diesen erst von erfahrenen Nutzern testen lassen. Erstelle dazu einen neuen `Foren-Beitrag <https://forum.iobroker.net/category/91/tester>`_ mit der Bitte um einen Test.

Danach kannst Du einen Pull-Request im `GitHub Repository (ioBroker.repositories) <https://github.com/ioBroker/ioBroker.repositories>`_ erstellen, indem Du Deinen Adapter dort hinzufügst. Mehr Details hier: :ref:`ecosystem-repositories`

.. note::
    Bitte beachte, dass Adapter abgelehnt werden, wenn nicht alle Adapter-Checks (siehe oben) erfüllt sind.

Hilfreiche Tools / Links
------------------------

- `Create-Adapter <https://github.com/ioBroker/create-adapter>`_
- `Adapter-Checker <https://adapter-check.iobroker.in/>`_
- `Release-Script von AlCalzone <https://github.com/AlCalzone/release-script>`_
- `Adapter-Examples <https://github.com/ioBroker/ioBroker.example>`_