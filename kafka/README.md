# Kafka Bindings

This document defines how to describe Kafka-specific information on AsyncAPI.

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

This object contains information about the operation representation in Kafka.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="operationBindingObjectGroupId"></a>`groupId` | string | Id of the consumer group.
<a name="operationBindingObjectClientId"></a>`clientId` | string | Id of the consumer inside a consumer group.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  user-signedup:
    publish:
      bindings:
        kafka:
          groupId:
            type: string
            enum: ['myGroupId']
          clientId:
            type: string
            enum: ['myClientId']
          bindingVersion: '0.1.0'
```


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in Kafka.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="messageBindingObjectKey"></a>`key` | [Schema Object][schemaObject] | The message key.
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.


```yaml
channels:
  test:
    publish:
      message:
        bindings:
          kafka:
            key:
              type: string
              enum: ['myKey']
            bindingVersion: '0.1.0'
```

[schemaObject]: https://www.asyncapi.com/docs/specifications/2.0.0/#schemaObject
