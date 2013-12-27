#node-netcat
	Arbitrary TCP and UDP connections and listens to be used in Node.js


##description
	**v0.0.1**
	Intention to implement all that "nc" allows and to be used in Node.js,
	for now can only open TCP connections and sending messages (Client), listen on arbitary TCP ports and response to the received messages (Server), and only deal with IPv4.


##Netcat -> 
	
	client(port, [host])
	
	client.send('data', [callback]);
	
	client.end([message])// can send a message and close the connection

	events: on('connect', function ())
			on('data', function (data))
			on('error', function (err))
			on('close', function ())
			
	##############################################
			
	server(port)
	
	server.close() // must not exists connections
	
	send data to client:
	1 - server.clients[client].end([message]) // send message and close connection
	2 - server.clients[client].write([message], [callback])
	
	events: on('ready', function ())
			on('data', function (data))
			on('error', function (err))
			on('close', function ())

##usage

**Client:**

	var Netcat = require('../')();
	
	var client = Netcat.client(5000);
	
	client.on('connect', function () {
	  console.log('connect');
	  client.send('this is a test');
	});
	
	client.on('data', function (data) {
	  console.log(data.toString('ascii'));
	  client.end([message]);
	});
	
	client.on('error', function (err) {
	  console.log(err);
	});
	
	client.on('close', function () {
	  console.log('close');
	});

**Server:**

	var Netcat = require('../')();
	
	var server = Netcat.server(5000);
	
	server.on('ready', function () { console.log('server ready'); });
	server.on('data', function (data) { console.log('server rx: ' + data); });
	server.on('error', function (err) { console.log(err); });
	server.on('close', function () { console.log('server closed'); });
	
	server.send('this is a test');
	
	// send messages to clients
	for (var client in server.clients) {
      server.clients[client].end('received ' + data);
    }
	