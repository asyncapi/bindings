# Kafka Bindings

This document defines how to describe Kafka-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.3.0`.


<a name="server"></a>

## Server Binding Object

This object contains information about the server representation in Kafka.

##### Fixed Fields

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|:---:|:---:|---
`schemaRegistryUrl` | string (url) | API URL for the Schema Registry used when producing Kafka messages (if a Schema Registry was used) | OPTIONAL | -
`schemaRegistryVendor` | string | The vendor of Schema Registry and Kafka serdes library that should be used (e.g. `apicurio`, `confluent`, `ibm`, or `karapace`) | OPTIONAL | MUST NOT be specified if `schemaRegistryUrl` is not specified
<a name="serverBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. | OPTIONAL [`latest`]

##### Example

```yaml
servers:
  production:
    bindings:
      kafka:
        schemaRegistryUrl: 'https://my-schema-registry.com'
        schemaRegistryVendor: 'confluent'
        bindingVersion: '0.3.0'
```


<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.


<a name="operation"></a>

## Operation Binding Object

This object contains information about the operation representation in Kafka.

##### Fixed Fields

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|:---:|:---:|---
<a name="operationBindingObjectGroupId"></a>`groupId` | [Schema Object][schemaObject] | Id of the consumer group. | OPTIONAL | -
<a name="operationBindingObjectClientId"></a>`clientId` | [Schema Object][schemaObject] | Id of the consumer inside a consumer group. | OPTIONAL | -
<a name="operationBindingObjectSchemaIdLocation"></a>`schemaIdLocation` | string | If a Schema Registry is used when performing this operation, tells where the id of schema is stored (e.g. `header` or `payload`). | OPTIONAL | MUST NOT be specified if `schemaRegistryUrl` is not specified at the Server level
<a name="operationBindingObjectSchemaIdPayloadEncoding"></a>`schemaIdPayloadEncoding` | string | Number of bytes or vendor specific values when schema id is encoded in payload (e.g `confluent`/ `apicurio-legacy` / `apicurio-new`). | OPTIONAL | MUST NOT be specified if `schemaRegistryUrl` is not specified at the Server level
<a name="operationBindingObjectSchemaLookupStrategy"></a>`schemaLookupStrategy` | string | Freeform string for any naming strategy class to use. Clients should default to the vendor default if not supplied. | OPTIONAL | MUST NOT be specified if `schemaRegistryUrl` is not specified at the Server level
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed. | OPTIONAL [`latest`] | -

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
          schemaIdLocation: 'payload'
          schemaIdPayloadEncoding: 'apicurio-new'
          schemaLookupStrategy: 'TopicIdStrategy'
          bindingVersion: '0.3.0'
```


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in Kafka.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="messageBindingObjectKey"></a>`key` | [Schema Object][schemaObject] \| [AVRO Schema Object](https://avro.apache.org/docs/current/spec.html) | The message key. **NOTE**: You can also use the [reference object](https://asyncapi.io/docs/specifications/v2.4.0#referenceObject) way.
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
            bindingVersion: '0.3.0'
```

[schemaObject]: https://www.asyncapi.com/docs/specifications/2.4.0/#schemaObject
