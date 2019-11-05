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
<a name="channelBindingObjectPayload"></a>`payload` | string | Defines the payload data type. Can be either `string`, `binary` or `JSON` (default). https://github.com/nats-io/nats.ts#connect-options



<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


unsub after n messages

sequence numbers

Request reply
    - timeout option
    -

queue groups



<a name="operation"></a>

## Operation Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.
