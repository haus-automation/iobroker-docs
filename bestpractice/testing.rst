.. _bestpractice-testing:

Testing
=======

Um verschiedene Tests für den eigenen ioBroker-Adapter vorzubereiten, gibt es das Paket `@iobroker/testing <https://github.com/ioBroker/testing>`_. Auf der GitHub-Seite sind einige Beispiele vorhanden.

Dabei werden folgende Typen unterschieden:

- Package-Tests prüfen, ob das Paket den generellen Anforderungen vom ioBroker-Team entspricht (ähnlich Adapter-Checker)
- JavaScript-Tests (laufen ohne einen ``js-controller``) und arbeiten mit Mocks
- Integration-Tests arbeiten mit einer ``js-controller``-Instanz, welche automatisch gestartet / verwaltet wird

Package-Tests
-------------

Diese Tests führen Prüfungen der ``package.json`` und der ``io-package.json`` durch. Datei ``test/package.js``:

.. code:: javascript

    const path = require('path');
    const { tests } = require('@iobroker/testing');

    // Validate the package files
    tests.packageFiles(path.join(__dirname, '..'));

JavaScript-Tests
----------------

Um Tests von einzelnen Dateien oder Klassen auszuführen, eigenen sich JavaScript-Tests, welche `Mocha <https://mochajs.org>`_ bzw. `Chai <https://www.chaijs.com>`_ Tests ausführen. Dabei wir eine zweite JavaScript-Datei neben die zu testende Datei gelegt, welche durch ein Namensschema automatisch als Test erkannt wird. Beispiel:

.. code::

    lib/convert.js
    lib/convert.test.js

Ein Test sieht dabei wie folgt aus:

.. code:: javascript

    const expect = require('chai').expect;
    const converter = require('../lib/converter');

    describe('Test conversions', function () {

        it('Test celsiusToFahrenheit', function () {
            expect(converter.celsiusToFahrenheit(15)).to.be.equal(59);
            expect(converter.celsiusToFahrenheit(0)).to.be.equal(32);
            expect(converter.celsiusToFahrenheit(100)).to.be.equal(212);
            expect(converter.celsiusToFahrenheit(-20)).to.be.equal(-4);
        });

        it('Test intToHex', function () {
            expect(converter.intToHex(0)).to.be.equal('00');
            expect(converter.intToHex(1)).to.be.equal('01');
            expect(converter.intToHex(15)).to.be.equal('0F');
            expect(converter.intToHex(255)).to.be.equal('FF');
        });

    });

Integration-Tests
-----------------

.. todo::
    Add Integration-Tests

Tests lokal ausführen
---------------------

Um alle Tests lokal auszuführen, können diese über npm angestoßen werden.

.. code:: console

    npm run test:js
    npm run test:integration
    npm run test:package

Damit diese Befehle funktionieren, muss die ``package.json`` des Adapters entsprechende Einträge enthalten:

.. code:: json

    "scripts": {
        "test:js": "mocha --config test/mocharc.custom.json \"{!(node_modules|test)/**/*.test.js,*.test.js,test/**/test!(PackageFiles|Startup).js}\"",
        "test:package": "mocha test/package --exit",
        "test:integration": "mocha test/integration --exit",
        "test": "npm run test:js && npm run test:package"
    }

GitHub Actions
--------------

.. todo::
    Add GitHub Actions

Links
-----

- `GitHub-Repo testing <https://github.com/ioBroker/testing>`_
- `GitHub-Repo testing-action-check (GitHub Actions) <https://github.com/ioBroker/testing-action-check>`_
- `GitHub-Repo testing-action-adapter (GitHub Actions) <https://github.com/ioBroker/testing-action-adapter>`_
- `GitHub-Repo testing-action-deploy (GitHub Actions) <https://github.com/ioBroker/testing-action-deploy>`_
- `test-and-release.yml Template <https://github.com/ioBroker/create-adapter/blob/master/templates/_github/workflows/test-and-release.yml.ts>`_
