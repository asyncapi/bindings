# NATS Bindings

This document defines how to describe NATS-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.1.0`.


<a name="server"></a>

## Server Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="serverBindingObjectPayload"></a>`payload` | string | Defines the payload data type. Can be either `binary`, `JSON` or `string` (default)




<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


##### Fixed Fields
Field Name | Type | Description
---|:---:|---
<a name="channelBindingObjectIs"></a>`is` | string | Defines what type of channel is it. Can be either `requestReply` or `pubsub` (default).
<a name="channelBindingObjectQueue"></a>`queue` | Map[string, any] | If the channel should use a queue, define the queue properties in this object.
<a name="channelBindingObjectQueueName"></a>`queue.name` | string | The name of the queue. It MUST NOT exceed 255 characters long.
<a name="channelBindingObjectRequestReply"></a>`requestReply` | Map[string, any] | If the channel is `requestReply`, define the request reply properties here.
<a name="channelBindingObjectRequestReplyTimeout"></a>`requestReply.timeout` | integer | The time you allow the subscriber to process the message and return a reply.




<a name="operation"></a>

## Operation Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.

Field Name | Type | Applies To | Description
---|:---:|:---:|---
<a name="operationBindingObjectUnsubAfter"></a>`unsubAfter` | integer | Subscribe | Defines whether the client should unsubscribe after n messages.

<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.
