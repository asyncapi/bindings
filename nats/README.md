# NATS Bindings

This document defines how to describe NATS-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `1.0.0`.

<a name="server"></a>

## Server Binding Object
This object MUST NOT contain any properties. Its name is reserved for future use.

<a name="channel"></a>

## Channel Binding Object

<a name="operation"></a>

##### Fixed Fields

| Field Name                                                                   |       Type       | Description                                                                             |
|------------------------------------------------------------------------------|:----------------:|-----------------------------------------------------------------------------------------|
| <a name="channelBindingObjectIs"></a>`is`                                    |      string      | Defines what type of channel it is. Can be either `requestReply` or `pubsub` (default). |
| <a name="channelBindingObjectRequestReply"></a>`requestReply`                | Map[string, any] | If the channel is `requestReply`, define the request reply properties here.             |
| <a name="channelBindingObjectRequestReplyTimeout"></a>`requestReply.timeout` |     integer      | The time you allow the replier to process the message and return a reply.            |
| <a name="channelBindingObjectRequestReplyIs"></a>`requestReply.is`           |      string      | Defines what type of request reply it is. Can be either `requester` or `replier`.       |
| <a name="channelBindingObjectBindingVersion"></a>`bindingVersion`            |      string      | The version of this binding. If omitted, "latest" MUST be assumed.                      |

## Operation Binding Object

<a name="message"></a>

| Field Name                                                          |       Type       |     Applies To     | Description                                                                                                                                                                                 |
|---------------------------------------------------------------------|:----------------:|:------------------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <a name="operationBindingObjectUnsubAfter"></a>`unsubAfter`         |     integer      |     Subscribe      | Defines whether the client should unsubscribe after n messages.                                                                                                                             |
| <a name="operationBindingObjectQueue"></a>`queue`                   | string |     Subscribe      | The name of the queue. It MUST NOT exceed 255 characters . Cannot be used used if channel `is` of type `requestReply` and `requestReply.is` are of type requester. |
| <a name="operationBindingObjectBindingVersion"></a>`bindingVersion` |      string      | Publish, Subscribe | The version of this binding. If omitted, "latest" MUST be assumed.                                                                                                                          |

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.
