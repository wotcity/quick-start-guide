<div class="row">
    <div class="col-md-12">
        <ol class="breadcrumb">
          <li><a href="#">Quick Start Guide</a></li>
          <li class="active">Develop Test Cases</li>
          <li class="active github"><a href="https://github.com/wotcity/quick-start-guide/tree/master/en-us/getting-started" class="fa fa-github">View on Github</a></li>
        </ol>
    </div>
</div>

# Develop Test Cases

## Frontend Functional Test: Real-time Data Push

The simplest way to test web frontend is use dummy websocket data. 

```
var WebSocketClient = require('websocket').client;

var client = new WebSocketClient();

client.on('connectFailed', function(error) {
    console.log('Connect Error: ' + error.toString());
});

client.on('connect', function(connection) {
    console.log('WebSocket client connected');
    connection.on('error', function(error) {
        console.log("Connection Error: " + error.toString());
    });
    connection.on('close', function() {
        console.log('echo-protocol Connection Closed');
    });
    connection.on('message', function(message) {
        if (message.type === 'utf8') {
            console.log("Received: '" + message.utf8Data + "'");
        }
    });
    function sendNumber() {
        if (connection.connected) {
            // The range of numbers is 1 to 100.
            var lucky = Math.round(Math.random() * 100 + 1);
            var obj = { temperature: lucky };

            console.log('Pushing: ' + JSON.stringify(obj));

            connection.sendUTF(JSON.stringify(obj));
            setTimeout(sendNumber, 1000);
        }
    }
    sendNumber();
});

client.connect('ws://wot.city/object/5550937980d51931b3000009/send', '');
```

Save above example code as *test.js*. How to execute *test.js*.

1. Run `$ npm i websocket` to install websocket module.
2. Run `$ node test.js` to start the program.

*test.js* sends a random temperature data every second.
