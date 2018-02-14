# Web Socket Connection to the Event Bus

## Overview

By connecting to the event bus where all the events occurring in the Cell flows, event information can be captured real time.
This API first requests its clients to send a valid access token. A session can be started only when 
the access token is valid and has required privilege. Then subscription condition should be specified next.
After subsription condition(s) is specified, events that match the condition(s) will be sent down to the clients.

### Required Privileges 

 event-read

## From Connection to the session start

### Endpoint URL

    wss:{UnitFQDN}/{CellName}/__event

By connectining to the above URL with Web Socket, it first becomes the status where access token can be accepted.
At this stage, any request other than sending access token is meaningless.

### Sending an Access Token

#### Request

By sending an access token in the following format, an event-bus connection session will be initiated.

    {"access_token":"AA~91WT0GNoVGFHJFQ.......e"}

#### Response

If the token is valid and has neccesary privilege, a response in the following format will be sent down to the client and the session starts. By starting the session, it becomes the state where events can be subscribed, however no evet will be sent down at this stage since no subscription is made yet.

    {"response":"success","expires_in":3600,"timestamp":1518612600}

If the token is invalid or does not have required privilege, the WebSocket connection will be closed by the Cell.

## Communication after the session starts

By sending a subscription condition, subscription will be made. Arbitrary number of multiple subscriptions can be made with different condition configurations. Upon an event occurence, the event information will be sent down to the client if it matches at least one of the subscription conditions.

### Event Subscription

#### Request

    {"subscribe": {"Type": "${EventType}", "Object": "${EventObject}"}}

#### Response

    {"response":"success","timestamp":1518612600}


### Unsubsribing events

#### Request

    {"unsubscribe": {"Type": "${EventType}", "Object": "${EventObject}"}}

#### Response

    {"response":"success","timestamp":1518612600}


### Receiving Events

If an event fires in the cell and the event matches one of the subscription conditions, a message in the following format will be sent down to the client.  


    {
      "Type":"chat", 
      "RequestKey":null,
      "Schema":"",
      "External":true,
      "Object":"general",
      "Info":"Hello World", 
      "cellId":"5ZKpfNSMSwO6GqAgt0O2Jg", 
      "Subject":"https:\/\/demo.personium.io\/john.doe\/#me"
    }


## Web Socket Spec Details

|Item|Spec|
|:--|:--|
|Protocol|Sec-WebSocket-Version 13(RFC 6455)|
|Message format|JSON|
|ping/pong|"ping" will be sent from the Cell every one minutes.|

* Connection will be closed if client fails to respond "pong" against the "ping" from the Cell 10 times.
* Major Web Browsers automatically responds "pong" in their WebSocket client implementations. 
