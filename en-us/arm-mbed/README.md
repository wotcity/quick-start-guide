<div class="row">
    <div class="col-md-12">
        <ol class="breadcrumb">
          <li><a href="#">Quick Start Guide</a></li>
          <li class="active">Develop IoT Device</li>
          <li class="active github"><a href="https://github.com/wotcity/quick-start-guide/tree/master/en-us/arm-mbed" class="fa fa-github">View on Github</a></li>
        </ol>
    </div>
</div>

# Develop IoT Device

WoT.City supports `ws://` service to connect physicals to HTML5 frontend.

## Usage

To send the data over the Internet, IoT devices should use the url below to establish a connection with the server.

```
ws://wot.city/object/[name]/send
```

You must specify a object name for the connection. For example:

```
ws://wot.city/object/554785c7173b2e5563000007/send
```

To receive data from the server, the frontend should use the url below to establish a connection with the server.

```
ws://wot.city/object/[name]/viewer
```

Also, you need to specify the object name. For example:

```
ws://wot.city/object/554785c7173b2e5563000007/viewer
```

An physical object has two significant resources, *send* and *viewer*. *send* is to send device data to WoT.City, and WoT.City receives data streams over Websocket connection. *viewer* is used by frontend to receive real-time data over the connection.

WoT.City is a broker that it receives sensor data over Websocket connection and routes data streams to frontend Websocket clients.

## Example: ARM mbed

Connect ARM mbed devices to WoT.City.

### Runtime

The full project is at [https://developer.mbed.org/users/jollen/code/Mokoversity_WoT_Wifly_Temperature/](https://developer.mbed.org/users/jollen/code/Mokoversity_WoT_Wifly_Temperature/).

* Arch Pro: ARM mbed IoT Kit
* WoT.City: Websocket broker


### Full Code

The example sends JSON data to `ws://wot.city` over Websocket.

```
int main() 
{
    char data[256];
    int ret;
 
    wait(3);
    
    //Use DHCP
    ret = eth.init(); 
    if (ret != NULL)
        exit(0);
    
    while (1) {
        ret = eth.connect();
        
        if (ret == false || ret < 0) {
            wait(1);
        } else {
            break;
        }
    }
    
    // Use WoT.City Websocket broker.
    Websocket ws("ws://wot.city/object/554785c7173b2e5563000007/send");
    while( !ws.connect() );
    
    while(1) { 
        wait(0.8);
        
        sprintf( data , "{ \"temperature\": %f }", 25.0);
        ws.send(data);
    }
}
```

## JSON Format

In this example, the JSON format is as following.

```
{
    "temperature": <Number>
}
```
