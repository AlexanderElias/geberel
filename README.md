# Geberel #

A simple IPC (inter process communicator) but really it is just WebSockets. Short simple and sweet.


## Server ##

```JavaScript
const Geberel = require('geberel');
const Server = Geberel.server;

const options = { port: 8000 };

Server(options, function (error, socket) {
	if (error) throw error;

	socket.receive('test', function (data, callback) {
		console.log(data); // { hello: 'people' }
		data.hello = 'world';
		return callback(data);
	});

	socket.transmit('another', 'cool thing');
});
```


## Client ##

```JavaScript
const Geberel = require('geberel');
const Client = Geberel.client;

const options = { address: 'ws://localhost:8000', autoClose: false };

Client(options, function (error, socket) {
	if (error) throw error;

	socket.transmit('test', { hello: 'people' }, function (data) {
		console.log(data); // { hello: 'world' }
	});

	socket.receive('another', function (data) {
		console.log(data); // cool thing
	});
});

```


## API ##

* **Geberel.client** - 'Options' `Object`, 'Callback' `Function`

	* **Callback** - 'Error' `Object`, 'Socket' `Object`

* **Geberel.server** - 'Options' `Object`, 'Callback' `Function`

	* **Callback** - 'Error' `Object`, 'Socket' `Object`

* **Socket.receive** - 'Event' `String`, 'Callback' `Function`

	* **Callback** - 'Data' `Object`, 'Callback' `Function` (optional)

* **Socket.transmit** - 'Event' `String`, 'Data' `Object`, 'Callback' `Function` (optional)

	* **Callback** - 'Data' `Object`


## Options ##

* Geberel.server
	* `port` Number **Default: 8000**
	* `host` String
	* `server` http.Server
	* `verifyClient` Function
	* `handleProtocols` Function
	* `path` String
	* `noServer` Boolean
	* `disableHixie` Boolean
	* `clientTracking` Boolean
	* `perMessageDeflate` Boolean|Object

* Geberel.client
	* `address` String **Default: ws://localhost:8000**
	* `autoClose` Boolean (closes all client sockets after completion) **Default: false**
	* `protocol` String
	* `agent` Agent
	* `headers` Object
	* `protocolVersion` Number|String
	-- the following only apply if address is a String
	* `host` String
	* `origin` String
	* `pfx` String|Buffer
	* `key` String|Buffer
	* `passphrase` String
	* `cert` String|Buffer
	* `ca` Array
	* `ciphers` String
	* `rejectUnauthorized` Boolean
	* `perMessageDeflate` Boolean|Object
	* `localAddress` String


## Terms ##

Basically if you modify this project you have to contribute those modifications back to this project.


## License ##

Licensed Under MPL 2.0

Copyright (c) 2016 [Alexander Elias](https://github.com/AlexanderElias/)
