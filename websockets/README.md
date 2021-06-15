# WebSockets Bindings

This document defines how to describe WebSockets-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.2.0`.


<a name="server"></a>

## Server Binding Object

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="operationBindingObjectQuery"></a>`query` | [Schema Object][schemaObject] | A Schema object containing the definitions for each query parameter to use when establishing the connection. This schema MUST be of type `object` and have a `properties` key.
<a name="operationBindingObjectHeaders"></a>`headers` | [Schema Object][schemaObject] | A Schema object containing the definitions of the HTTP headers to use when establishing the connection. This schema MUST be of type `object` and have a `properties` key.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.



<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="operation"></a>

## Operation Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


[schemaObject]: https://www.asyncapi.com/docs/specifications/2.0.0/#schemaObject