.. _development-messagebox:

Messagebox
==========

Adapter können Informationen auf unterschiedliche Arten austauschen. Gängig ist dabei der Weg über Objekte/Zustände, sodass ein Adpater in einen Datenpunkt schreibt und ein anderer diesen Abonniert hat (subscribe), um über Änderungen informiert zu werden.

Neben dieser Variante, können Adapter aber noch eine weitere Schnittstelle anbieten: Die sog. "Message Box". Ist dieses Feature aktiviert, kann über ``sendTo`` eine andere Instanz mit Infos versorgt werden. Dies findet in vielen Adpatern anwendung.

Konfiguration in der :ref:`development-iopackage` (``common.supportedMessages``)



.. todo::
    Add Messagebox

    REST-API: http://ipaddress:8093/v1/sendto/javascript.0?message=toScript&data={"message":"MESSAGE","data":"FROM REST-API"}
    CLI
