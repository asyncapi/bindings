# Server Sent Events Bindings

This document defines how to describe Server Sent Events-specific information on AsyncAPI.

See the [Server-Sent Events protocol specification][protocolSpecification].

<a name="version"></a>

## Version

Current version is `0.1.0`.


<a name="server"></a>

## Server Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="channel"></a>

## Channel Binding Object

When using Server Sent Events, the channel represents a single logical message stream flowing from server to client. 

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="operationBindingObjectMethod"></a>`method` | string | The HTTP method to use when establishing the request. Its value MUST be `GET`.
<a name="operationBindingObjectQuery"></a>`query` | [Schema Object][schemaObject] \| [Reference Object](referenceObject) | A Schema object containing the definitions for each query parameter. This schema MUST be of type `object` and have a `properties` key.
<a name="operationBindingObjectHeaders"></a>`headers` | [Schema Object][schemaObject] \| [Reference Object](referenceObject) | A Schema object containing the definitions of the HTTP headers to use when establishing the request. This schema MUST be of type `object` and have a `properties` key.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

<a name="operation"></a>

## Operation Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in SSE.

##### Fixed Fields

Field Name | Type | Description
---|:---:|:---:|---
<a name="messageBindingObjectEventType"></a>`event` | string | Server-sent event type, defaults to `message` if omitted, by the [SSE specification](protocolSpecification). |
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.


[schemaObject]: https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#schemaObject
[referenceObject]: https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#referenceObject
[protocolSpecification]: https://html.spec.whatwg.org/multipage/server-sent-events.html#server-sent-events
