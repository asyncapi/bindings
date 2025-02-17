# Server Sent Events Bindings

This document defines how to describe Server Sent Events-specific information on AsyncAPI.

See the [Server-Sent Events protocol specification][protocolSpecification].

Server-Sent Events requires `http` protocol, so the SSE [Message Binding Object](#message) below is appropriate for AsyncAPI Servers with protocol `http` or `https`.

<a name="version"></a>

## Version

Current version is `0.1.0`.


<a name="server"></a>

## Server Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="operation"></a>

## Operation Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in SSE.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="messageBindingObjectEventType"></a>`event` | string | Server-sent event type, typically `message`, to specify allowed SSE message types. |
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.


[protocolSpecification]: https://html.spec.whatwg.org/multipage/server-sent-events.html#server-sent-events
