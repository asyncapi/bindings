# Kafka Bindings

This document defines how to describe Kafka-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.2.0`.


<a name="server"></a>

## Server Binding Object

This object contains information about the server representation in Kafka.

##### Fixed Fields

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|:---:|:---:|---
`schemaRegistryUrl` | string (url) | API URL for the Schema Registry used when producing Kafka messages (if a Schema Registry was used) | OPTIONAL | -
`schemaRegistryVendor` | string | Identifies the Kafka serdes library that should be used (e.g. `confluent`, `apicurio`, or `ibm`) | OPTIONAL | MUST NOT be specified if `schemaRegistryUrl` is not specified
`schemaRegistryAvailable` | boolean | Specifies if the Schema Registry identified in `schemaRegistryUrl` is available for use by consumers of the AsyncAPI spec | OPTIONAL [true] | MUST NOT be specified if `schemaRegistryUrl` is not specified
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. | OPTIONAL [`latest`]

##### Example

```yaml
servers:
  production:
    bindings:
      kafka:
        schemaRegistryUrl: "https://my-schema-registry.com"
        schemaRegistryVendor: "confluent"
        schemaRegistryAvailable: false
        bindingVersion: '0.2.0'
```


<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="operation"></a>

## Operation Binding Object

This object contains information about the operation representation in Kafka.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="operationBindingObjectGroupId"></a>`groupId` | [Schema Object][schemaObject] | Id of the consumer group.
<a name="operationBindingObjectClientId"></a>`clientId` | [Schema Object][schemaObject] | Id of the consumer inside a consumer group.
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
          bindingVersion: '0.2.0'
```


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in Kafka.

##### Fixed Fields

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|:---:|:---:|---
<a name="messageBindingObjectKey"></a>`key` | [Schema Object][schemaObject] | The message key. | OPTIONAL | -
`schemaIdLocation` | `none` or `header` or `payload` | Location in the message of the ID that identifies the schema in a schema registry.  | OPTIONAL [`none`] | -
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. | OPTIONAL [`latest`] | -

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
            schemaIdLocation: 'payload'
            bindingVersion: '0.2.0'
```

[schemaObject]: https://www.asyncapi.com/docs/specifications/2.0.0/#schemaObject
