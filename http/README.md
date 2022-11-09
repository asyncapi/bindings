# HTTP Bindings

This document defines how to describe HTTP-specific information on AsyncAPI.

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

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="operationBindingObjectType"></a>`type` | string | **REQUIRED**. Type of operation. Its value MUST be either `request` or `response`.
<a name="operationBindingObjectMethod"></a>`method` | string | When `type` is `request`, this is the HTTP method, otherwise it MUST be ignored. Its value MUST be one of `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS`, `CONNECT`, and `TRACE`.
<a name="operationBindingObjectQuery"></a>`query` | [Schema Object][schemaObject] | A Schema object containing the definitions for each query parameter. This schema MUST be of type `object` and have a `properties` key.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  /employees:
    subscribe:
      bindings:
        http:
          type: request
          method: GET
          query:
            type: object
            required:
              - companyId
            properties:
              companyId:
                type: number
                minimum: 1
                description: The Id of the company.
            additionalProperties: false
          bindingVersion: '0.1.0'
```


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in HTTP.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="messageBindingObjectHeaders"></a>`headers` | [Schema Object][schemaObject] | A Schema object containing the definitions for HTTP-specific headers. This schema MUST be of type `object` and have a `properties` key.
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.


```yaml
channels:
  test:
    publish:
      message:
        bindings:
          http:
            headers:
              type: object
              properties:
                Content-Type:
                  type: string
                  enum: ['application/json']
            bindingVersion: '0.1.0'
```

[schemaObject]: https://www.asyncapi.com/docs/specifications/2.0.0/#schemaObject