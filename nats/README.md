# NATS Bindings

This document defines how to describe NATS-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.1.0`.

<a name="server"></a>

## Server Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.

##### Fixed Fields

| Field Name                                                       |  Type  | Description                                                        |
| ---------------------------------------------------------------- | :----: | ------------------------------------------------------------------ |
| <a name="serverBindingObjectEncoding"></a>`encoding`             | string | Defines the payloads encoding. Default to `utf8`.                  |
| <a name="serverBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed. |

<a name="channel"></a>

## Channel Binding Object

<a name="operation"></a>

This object MUST NOT contain any properties. Its name is reserved for future use.

##### Fixed Fields

| Field Name                                                                   |       Type       | Description                                                                             |
|------------------------------------------------------------------------------|:----------------:|-----------------------------------------------------------------------------------------|
| <a name="channelBindingObjectIs"></a>`is`                                    |      string      | Defines what type of channel it is. Can be either `requestReply` or `pubsub` (default). |
| <a name="channelBindingObjectRequestReply"></a>`requestReply`                | Map[string, any] | If the channel is `requestReply`, define the request reply properties here.             |
| <a name="channelBindingObjectRequestReplyTimeout"></a>`requestReply.timeout` |     integer      | The time you allow the subscriber to process the message and return a reply.            |
| <a name="channelBindingObjectRequestReplyIs"></a>`requestReply.is`           |      string      | Defines what type of request reply it is. Can be either `requester` or `replier`.       |
| <a name="channelBindingObjectBindingVersion"></a>`bindingVersion`            |      string      | The version of this binding. If omitted, "latest" MUST be assumed.                      |

## Operation Binding Object

<a name="message"></a>
This object MUST NOT contain any properties. Its name is reserved for future use.

| Field Name                                                          |       Type       |     Applies To     | Description                                                                                                                                                                                 |
|---------------------------------------------------------------------|:----------------:|:------------------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <a name="operationBindingObjectUnsubAfter"></a>`unsubAfter`         |     integer      |     Subscribe      | Defines whether the client should unsubscribe after n messages.                                                                                                                             |
| <a name="operationBindingObjectQueue"></a>`queue`                   | Map[string, any] |     Subscribe      | If the subscription should use a queue, define the queue properties in this object. Cannot be used used if channel `is` of type `requestReply` and `requestReply.is` are of type requester. |
| <a name="operationBindingObjectQueueName"></a>`queue.name`          |      string      |     Subscribe      | The name of the queue. It MUST NOT exceed 255 characters long.                                                                                                                              |
| <a name="operationBindingObjectBindingVersion"></a>`bindingVersion` |      string      | Publish, Subscribe | The version of this binding. If omitted, "latest" MUST be assumed.                                                                                                                          |

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.
