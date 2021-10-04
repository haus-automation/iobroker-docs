.. _development-testing:

Testing
=======

Um verschiedene Tests für den eigenen ioBroker-Adapter vorzubereiten, gibt es das Paket `@iobroker/testing <https://github.com/ioBroker/testing>`_. Auf der GitHub-Seite sind einige Beispiele vorhanden.

Dabei werden folgende Typen unterschieden:

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

Unit-Test
---------

Diese Tests führen allgemeine Unit-Tests durch. Datei ``test/unit.js``:

.. code:: javascript

    const path = require('path');
    const { tests } = require('@iobroker/testing');

    // Run unit tests - See https://github.com/ioBroker/testing for a detailed explanation and further options
    tests.unit(path.join(__dirname, '..'));

Tests ausführen
---------------



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

