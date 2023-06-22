# WebSockets Bindings

This document defines how to describe WebSockets-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.2.0`.

<a name="server"></a>

## Server Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.

<a name="channel"></a>

## Channel Binding Object

When using WebSockets, the channel represents the connection. Unlike other protocols that support multiple virtual channels (topics, routing keys, etc.) per connection, WebSockets doesn't support virtual channels or, put it another way, there's only one channel and its characteristics are strongly related to the protocol used for the handshake, i.e., HTTP.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="operationBindingObjectMethod"></a>`method` | string | The HTTP method to use when establishing the connection. Its value MUST be either `GET` or `POST`.
<a name="operationBindingObjectQuery"></a>`query` | [Parameters Object][parametersObject] | A Schema object containing the definitions for each query parameter. 
<a name="operationBindingObjectHeaders"></a>`headers` | [Parameters Object][parametersObject] | A Schema object containing the definitions of the HTTP headers to use when establishing the connection.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

<a name="operation"></a>

## Operation Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.

<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


[parametersObject]: https://www.asyncapi.com/docs/specifications/latest/#parametersObject