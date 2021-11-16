.. _bestpractice-logging:

Logging
=======

Bei der Adapter-Entwicklung sollte man darauf achten, dass man aussagekräftige Logs schreibt. Dabei gilt es jeweilige Log-Level zu berücksichtigen. Je nach eingestelltem Loglevel auf der aktuellen Instanz, werden unterschiedlich viele Informationen geloggt.

Das Standard-Loglevel eines Adapters / neuer Instanzen wird in der Konfiguration in der :ref:`development-iopackage` festgelegt (``common.loglevel``). Dieses Level kann später vom Benutzer auf jeder einzelnen Instanz angepasst werden!

.. code:: javascript

    this.log.debug('debug message'); // log message with debug level
    this.log.info('info message');   // log message with info level (enabled by default for all adapters)
    this.log.warn('warning');        // log message with info warn
    this.log.error('error');         // log message with info error

