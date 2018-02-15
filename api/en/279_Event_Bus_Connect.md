# Web Socket Connection to the Event Bus

## Overview

By connecting to the event bus where all the events occurring in the Cell flow, these events information can be captured in real time.
This API first requests its client to send a valid access token. A session can be started only when 
the access token is valid and has required privileges. Then after the session is established, subscription conditions can be specified at any moment. After a subsription condition is specified, events that match any of the conditions will be sent down to the client. More subscription conditions can be added anytime to subscribe more events. Also any of the subscriptions can be canceled anytime by sending  unsubscribing messeages.

### Required Privilege 

 event-read

## Connection and starting the session

### Connection Endpoint URL

    wss:{UnitFQDN}/{CellName}/__event

By connectining to the above URL with Web Socket, it first goes to the status where an access token can be accepted.
At this stage, any request other than sending access token is meaningless.

### Starting the session by sending an access token

#### Request Message

By sending an access token in the following format, an event-bus connection session will be initiated.

    {"access_token":"AA~91WT0GNoVGFHJFQ.......e"}

#### Response Message

If the token is valid and has neccesary privilege, a response in the following format will be sent down to the client and the session starts. By starting the session, it becomes the state where events can be subscribed, however no evet will be sent down at this stage since no subscription is made yet.

    {"response":"success","expires_in":3600,"timestamp":1518612600}

If the token is invalid or does not have the required privilege, the WebSocket connection will be closed by the Cell.

## Communication after the session is establised

After the session is establised, the following 4 commands can be sent from the client as request messages.

1. Event Subscription
1. Event Unsubscription
1. Session State Retrieval
1. Access Token Re-sending

By sending a subscription condition, subscription will be made. Arbitrary number of multiple subscriptions can be made with different condition configurations. Upon an event occurence, the event information will be sent down to the client if it matches at least one of the subscription conditions. By session state retrieval, information such as current subscriptions status or session remaining time can be retrieved. Access token resending enables prolongation of the session expiry time.

When a request message for one of these commands is sent, then a response message will promptly be returned.
If the request message has any trouble, then an error response message will be returned.

## Event Subscription

### Request Message

    {"subscribe": {"Type": "${EventType}", "Object": "${EventObject}"}}

* For both Type and Object, matching is made by front match.
* For both Type and Object, specify * to let them match arbitrary values.

### Response Message

    {"response":"success","timestamp":1518612600}


## Receiving Events

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

## Session State Retrieval

### Request Message

Retrieving substcriptions state

    {"state": "subscriptions"}

Retrieving all the sesseion state information 

    {"state": "all"}

### Response Message

On retrieving substcriptions state

    {response: "success", subscriptions: [] }

On retrieving all the sesseion state information 

    {response: "success", cell: ${cell_name}, expires_in: 2986, subscriptions: [] }


## Unsubsribing events

#### Request Message

    {"unsubscribe": {"Type": "${EventType}", "Object": "${EventObject}"}}

#### Response Message

    {"response":"success","timestamp":1518612600}

## Error Response Messages

When command message format is invalid

    {response: "error", reason: "format error"}

When trying to unsbscribe non-existent subscription condition

    {response: "error", reason: "subscriptions not found"}


## Web Socket Spec Details

|Item|Spec|
|:--|:--|
|Protocol|Sec-WebSocket-Version 13(RFC 6455)|
|Message format|JSON|
|ping/pong|"ping" will be sent from the Cell every one minutes.|

* Connection will be closed if client fails to respond "pong" against the "ping" from the Cell 10 times.
* Major Web Browsers automatically responds "pong" in their WebSocket client implementations. 
