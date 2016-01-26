# heartbeat

A heartbeat monitor.

This module is based off [jasonkuhrt-heartbeat](https://github.com/jasonkuhrt/heartbeat) 
but differes in that the heartbeat will continue to call the flatlinefunction unless there 
has been at least 1 call to the heartbeat.

Its designed to monitor data streams on an open connection for some form of state. Where a
connection being open isn't enough to determine if something is healthy.


## Installation

    npm install jasonkuhrt-heartbeat

## Example
```js
var tcp = require('net');
var heartbeat = require('prime-heartbeat');

var thump = heartbeat(onFlatLine,500);

var socket = new net.Socket();
socket.connect(3000,'127.0.0.1');

socket.on('data', thump);

function onFlatLine() {
  console.log('Remote device isnt sending any data');
}
```
```
> npm start
...
... (Socket opens, streams in data. After some period the remote device misbehaves)
...
Remote device isnt sending any data
```

## API

#### Heartbeat → .setHeartbeat


#### .setHeartbeat(onFlatline, intervalMs)
    Heartbeat h; Int i;  :: ( -> ), i -> h

  Returns a heartbeat instance. A heartbeat instance is a function. Invoke it to keep the heartbeat going. The identifier is typically `thump` (see guide).

  - `onFlatline` is invoked  and will continue to invoke when/if `thump` is *not* invoked during an interval until cleared.

  - `intervalMs` sets the time between thump checks.



#### .clear(heartbeat)

    Heartbeat a :: a -> undefined

  Destroy a heartbeat instance. Analagous to `clearInterval`/`clearTimeout`.

  Only use `clearHeartbeat` if you need to abort a heartbeat before `onFlatline`.


## Guide

https://github.com/probableprime/heartbeat/blob/master/test/index.js
