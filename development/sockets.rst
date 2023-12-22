.. _development-sockets:

Sockets
=======

Um mit dem ioBroker über Websockets zu kommunizieren, gibt es verschiedene Wege. Im Standard stellen sowohl der Admin- und der Web-Adapter eine Schnittstelle bereit, welche Websocket-Verbindungen annehmen.

Adapter (Server-Seite)
----------------------

- https://github.com/ioBroker/ioBroker.socketio (sollte nicht mehr verwendet werden?)
- https://github.com/ioBroker/ioBroker.ws (bevorzugt!)

Socket-Libs
-----------

https://github.com/ioBroker/socket-client `@iobroker/socket-client`

genutzt von
    https://github.com/ioBroker/adapter-react-v5/

Legacy-Libs
-----------

Die älteren Confings, welche HTML oder Materialze, nutzen im Admin diese Libs:

Server: https://github.com/ioBroker/ioBroker.ws.server `@iobroker/ws-server`

genutzt von
    Admin-Adapter

Client: https://github.com/ioBroker/ioBroker.ws.client `@iobroker/ws`

genutzt von
    Web-Adapter
    Materialize-Configs: `<script type="text/javascript" src="../../socket.io/socket.io.js"></script>`
    Scheinbar genau das gleiche wie: `<script type="text/javascript" src="./../../lib/js/socket.io.js"></script>`

Weiteres
--------

https://github.com/ioBroker/webserver (für Zertifikats-Management aus der Adapter-Config - public, private, chained, ...)

genutzt von
    Admin-Adapter




Der Web-Adapter https://github.com/ioBroker/ioBroker.web nutzt `ioBroker.socketio` oder `ioBroker.ws` (usePureWebSockets = "reine Websockets verwenden") und `ioBroker/webserver`!
