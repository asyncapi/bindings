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

#### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="operationBindingObjectMethod"></a>`method` | string | The HTTP method to use when establishing the connection. Its value MUST be either `GET` or `POST`.
<a name="operationBindingObjectQuery"></a>`query` | [Schema Object][schemaObject] | A Schema object containing the definitions for each query parameter. This schema MUST be of type `object` and have a `properties` key.
<a name="operationBindingObjectHeaders"></a>`headers` | [Schema Object][schemaObject] | A Schema object containing the definitions of the HTTP headers to use when establishing the connection. This schema MUST be of type `object` and have a `properties` key.
<a name="operationBindingObjectQueryValues"></a>`queryValues`| object| An object containing query values.
<a name="operationBindingObjectHeaderValues"></a>`headerValues`| object | An object containing header values.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

#### Passing ENV variables to headers and query 

All string values containing words starting with the '$' sign, MUST be replaced with environment variables of the same name at runtime. The following rules MUST be applied:

- Environment variables' names MUST use uppercase letters (A-Z), underscore characters (_), or digits (0-9).
- Environment variables' names SHALL NOT start with a digit (0-9) or an underscore character (_).

##### Do's 
- `$TOKEN`
- `$GITHUB_TOKEN`
- `$TOKEN1`
##### Don'ts 
- `$ TOKEN`
- `$1TOKEN`
- `$token`
- `$_TOKEN`


## Example

```yaml
channels:
    /employees:
        bindings:
            ws:
                bindingVersion: 0.2.0
                headers:
                    type: object
                    required: [
                        'Authorization'
                    ]
                    properties:
                        Authorization:
                            type: string
                query:
                    type: object
                    requried: [
                        'employeeId'
                    ]
                    properties:
                        employeeId: string
                headerValues:
                    Authentication: 'bearer $TOKEN'
                queryValues:
                    employeeId: '$EMP_ID'
        publish:
            message:
                $ref: '#/components/messages/employeeData'
        subscribe:
            message:
                $ref: '#/components/messages/employeeData'
```

<a name="operation"></a>

## Operation Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


[schemaObject]: https://www.asyncapi.com/docs/specifications/2.0.0/#schemaObject