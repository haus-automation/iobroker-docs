.. _bestpractice-testing:

Testing
=======

Um verschiedene Tests für den eigenen ioBroker-Adapter vorzubereiten, gibt es das Paket `@iobroker/testing <https://github.com/ioBroker/testing>`_. Auf der GitHub-Seite sind einige Beispiele vorhanden.

Dabei werden folgende Typen unterschieden:

- Package-Tests prüfen, ob das Paket den generellen Anforderungen vom ioBroker-Team entspricht (ähnlich Adapter-Checker)
- Unit-Tests (laufen ohne einen ``js-controller``) und arbeiten mit Mocks
- Integration-Tests arbeiten mit einer ``js-controller``-Instanz, welche automatisch gestartet wird

Package-Tests
-------------

Diese Tests führen Prüfungen der ``package.json`` und der ``io-package.json`` durch. Datei ``test/package.js``:

.. code:: javascript

    const path = require('path');
    const { tests } = require('@iobroker/testing');

    // Validate the package files
    tests.packageFiles(path.join(__dirname, '..'));

Integration-Tests
-----------------

<!-- TODO: Add Integration-Tests -->

Unit-Test
---------

Diese Tests führen allgemeine Unit-Tests durch. Datei ``test/unit.js``:

.. code:: javascript

    const path = require('path');
    const { tests } = require('@iobroker/testing');

    // Run unit tests - See https://github.com/ioBroker/testing for a detailed explanation and further options
    tests.unit(path.join(__dirname, '..'));

Tests lokal ausführen
---------------------

Um alle Tests lokal auszuführen, können diese über npm angestoßen werden.

.. code:: console

    npm run test:js
    npm run test:integration
    npm run test:package
    npm run test:unit

Damit diese Befehle funktionieren, muss die ``package.json`` des Adapters entsprechende Einträge enthalten:

.. code:: json

    "scripts": {
        "test:js": "mocha --config test/mocharc.custom.json \"{!(node_modules|test)/**/*.test.js,*.test.js,test/**/test!(PackageFiles|Startup).js}\"",
        "test:package": "mocha test/package --exit",
        "test:unit": "mocha test/unit --exit",
        "test:integration": "mocha test/integration --exit",
        "test": "npm run test:js && npm run test:package"
    }

GitHub Actions
--------------

<!-- TODO: Add GitHub Actions -->

Links
-----

- `GitHub-Repo testing <https://github.com/ioBroker/testing>`_
- `GitHub-Repo testing-action-check (GitHub Actions) <https://github.com/ioBroker/testing-action-check>`_
- `GitHub-Repo testing-action-adapter (GitHub Actions) <https://github.com/ioBroker/testing-action-adapter>`_
- `GitHub-Repo testing-action-deploy (GitHub Actions) <https://github.com/ioBroker/testing-action-deploy>`_
- `test-and-release.yml Template <https://github.com/ioBroker/create-adapter/blob/master/templates/_github/workflows/test-and-release.yml.ts>`_
