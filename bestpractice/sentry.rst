.. _bestpractice-sentry:

Sentry
=======

Um als Entwickler eigene Nachrichten an :ref:`ecosystem-sentry` zu generieren, muss der Adapter für diesen Dienst freigeschaltet werden und ein DSN in der :ref:`development-iopackage` gepflegt sein.

Der "Standard-Scope" enthält vom Plugin bereits Informationen wie

- die Adapter-Version
- die nodejs-Version
- das genutzt Betriebssysstem
- welche Engine für die Datenbanken genutzt wird
- Die UUID des Systems - siehe :ref:`basics-uuid`

Beispiel:

.. code:: javascript

    if (this.supportsFeature && this.supportsFeature('PLUGINS')) {
        const sentryInstance = this.getPluginInstance('sentry');
        if (sentryInstance) {
            const Sentry = sentryInstance.getSentryObject();

            Sentry && Sentry.withScope(scope => {
                // Available levels are "fatal", "error", "warning", "log", "info", and "debug".
                scope.setLevel(Sentry.Severity.Warning);

                scope.setExtra('key', 'value');
                Sentry.captureMessage('Event name', 'warning');
            });
        }
    }

Links
-----

- `sentry.iobroker.net <https://sentry.iobroker.net/>`_
- `Sentry-Plugin Repository <https://github.com/ioBroker/plugin-sentry>`_ - ``@iobroker/plugin-sentry``
- `Plugin-Base Repository <https://github.com/ioBroker/plugin-base>`_ - ``@iobroker/plugin-base``
- `Sentry Dokumentation <https://docs.sentry.io/platforms/javascript/>`_
