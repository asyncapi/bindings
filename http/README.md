# HTTP Bindings

This document defines how to describe HTTP-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.2.0`.


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
<a name="operationBindingObjectMethod"></a>`method` | string | The HTTP method for the request. Its value MUST be one of `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS`, `CONNECT`, and `TRACE`.
<a name="operationBindingObjectQuery"></a>`query` | [Parameters Object][parametersObject] | A Parameters object containing the definitions for each query parameter.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  employees:
    address: /employees
operations:
  employees:
    action: send:
    bindings:
      http:
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
        bindingVersion: '0.2.0'
```


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in HTTP.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="messageBindingObjectHeaders"></a>`headers` | [Parameters Object][parametersObject] | A Parameters object containing the definitions for HTTP-specific headers.
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.


```yaml
channels:
  test:
    address: /test
    messages:
      testMessage:
        bindings:
          http:
            headers:
              type: object
              properties:
                Content-Type:
                  type: string
                  enum: ['application/json']
            bindingVersion: '0.2.0'
```

[schemaObject]: https://www.asyncapi.com/docs/specifications/latest/#schemaObject
[parametersObject]: https://www.asyncapi.com/docs/specifications/latest/#parametersObject