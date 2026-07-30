# Kafka Bindings

This document defines how to describe Kafka-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.6.0`.


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
        bindingVersion: '0.6.0'
```


<a name="channel"></a>

## Channel Binding Object

This object contains information about the channel representation in Kafka (eg. a Kafka topic).

##### Fixed Fields

Field Name |                       Type                       |                                               Description                                               | Applicability [default] | Constraints
---|:------------------------------------------------:|:-------------------------------------------------------------------------------------------------------:|:-----------------------:|---
<a name="channelBindingObjectTopic"></a>`topic` |                      string                      |                            Kafka topic name if different from channel name.                             |        OPTIONAL         | -
<a name="channelBindingObjectPartitions"></a>`partitions` |                     integer                      | Number of partitions configured on this topic (useful to know how many parallel consumers you may run). |        OPTIONAL         | Must be positive
<a name="channelBindingObjectReplicas"></a>`replicas` |                     integer                      |                              Number of replicas configured on this topic.                               |        OPTIONAL         | MUST be positive
<a name="channelBindingObjectTopicConfiguration"></a>`topicConfiguration` | [TopicConfiguration Object](#topicConfiguration) |                   Topic configuration properties that are relevant for the API.                    |       OPTIONAL       | -
<a name="channelBindingObjectEnvServerOverrides"></a>`envServerOverrides` | Map[string, [Partial Channel Binding Object](#channelBindingObject)] | A map of server name (as defined in the AsyncAPI document) to a partial channel binding object. When the named server is active, its overrides are merged onto the base channel binding. Allows the same AsyncAPI document to declare environment-specific topic settings (e.g. reduced partitions and replicas in dev) without duplicating channel definitions. | OPTIONAL | Keys MUST match server names defined in the AsyncAPI document. Values MUST only contain properties defined for the Channel Binding Object, excluding `envServerOverrides` and `bindingVersion`. |
<a name="channelBindingObjectBindingVersion"></a>`bindingVersion` |                      string                      |                   The version of this binding. If omitted, "latest" MUST be assumed.                    |   OPTIONAL [`latest`]   | -


This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  user-signedup:
    bindings:
      kafka:
        topic: 'my-specific-topic-name'
        partitions: 20
        replicas: 3
        topicConfiguration:
          cleanup.policy: ["delete", "compact"]
          retention.ms: 604800000
          retention.bytes: 1000000000
          delete.retention.ms: 86400000
          max.message.bytes: 1048588
        bindingVersion: '0.6.0'
```

```yaml
channels:
  payment-events:
    bindings:
      kafka:
        topic: 'payment-events'
        partitions: 12
        replicas: 3
        topicConfiguration:
          cleanup.policy: ["delete"]
          retention.ms: 604800000
        envServerOverrides:
          dev:
            partitions: 1
            replicas: 1
            topicConfiguration:
              cleanup.policy: ["delete"]
              retention.ms: 60000
          staging:
            partitions: 3
            replicas: 2
        bindingVersion: '0.6.0'
```
<a name="topicConfiguration"></a>
## TopicConfiguration Object

This objects contains information about the API relevant topic configuration in Kafka.

Field Name |  Type   |                                                                             Description                                                                              | Applicability [default] | Constraints
---|:-------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------:|---
<a name="topicConfigurationCleanupPolicy"></a>`cleanup.policy` |  array  |                            The [`cleanup.policy`](https://kafka.apache.org/documentation/#topicconfigs_cleanup.policy) configuration option.                          |        OPTIONAL         | array may only contain `delete` and/or `compact`
<a name="topicConfigurationRetentionMs"></a>`retention.ms` | long |                               The [`retention.ms`](https://kafka.apache.org/documentation/#topicconfigs_retention.ms) configuration option.                                    |        OPTIONAL         | see kafka documentation
<a name="topicConfigurationRetentionBytes"></a>`retention.bytes` | long |                       The [`retention.bytes`](https://kafka.apache.org/documentation/#topicconfigs_retention.bytes) configuration option.                                                           |        OPTIONAL         | see kafka documentation
<a name="topicConfigurationDeleteRetentionBytes"></a>`delete.retention.ms` | long |             The [`delete.retention.ms`](https://kafka.apache.org/documentation/#topicconfigs_delete.retention.ms) configuration option.                                               |        OPTIONAL         | see kafka documentation
<a name="topicConfigurationMaxMessageBytes"></a>`max.message.bytes` | integer |                    The [`max.message.bytes`](https://kafka.apache.org/documentation/#topicconfigs_max.message.bytes) configuration option.                                      |        OPTIONAL         | see kafka documentation
<a name="topicConfigurationConfluentKeySchemaValidation"></a>`confluent.key.schema.validation`  | boolean |                    It shows whether the schema validation for the message key is enabled. Vendor specific config.                                      |        OPTIONAL         | -
<a name="topicConfigurationConfluentKeySubjectNameStrategy"></a>`confluent.key.subject.name.strategy` | string |                    The name of the schema lookup strategy for the message key. Vendor specific config.                                     |        OPTIONAL         | Clients should default to the vendor default if not supplied.
<a name="topicConfigurationConfluentValueSchemaValidation"></a>`confluent.value.schema.validation` | boolean |                    It shows whether the schema validation for the message value is enabled. Vendor specific config.                                      |        OPTIONAL         | -
<a name="topicConfigurationConfluentValueSubjectNameStrategy"></a>`confluent.value.subject.name.strategy` | string |                    The name of the schema lookup strategy for the message value. Vendor specific config.                                      |        OPTIONAL         | Clients should default to the vendor default if not supplied.

This object MAY contain the properties defined above including optional additional properties.

##### Example

```yaml
topicConfiguration:
  cleanup.policy: ["delete", "compact"]
  retention.ms: 604800000
  retention.bytes: 1000000000
  delete.retention.ms: 86400000
  max.message.bytes: 1048588
  confluent.key.schema.validation: true
  confluent.key.subject.name.strategy: "TopicNameStrategy"
  confluent.value.schema.validation: true
  confluent.value.subject.name.strategy: "TopicNameStrategy"
```

<a name="operation"></a>

## Operation Binding Object

This object contains information about the operation representation in Kafka (eg. the way to consume messages)

##### Fixed Fields

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|:---:|:---:|---
<a name="operationBindingObjectGroupId"></a>`groupId` | [Schema Object][schemaObject] \| [Reference Object](referenceObject) | Id of the consumer group. | OPTIONAL | -
<a name="operationBindingObjectClientId"></a>`clientId` | [Schema Object][schemaObject] \| [Reference Object](referenceObject) | Id of the consumer inside a consumer group. | OPTIONAL | -
<a name="operationBindingObjectPrincipal"></a>`principal` | string | The Kafka principal (e.g. `User:bob`) used to authenticate this operation. Intended for ACL documentation purposes. | OPTIONAL | -
<a name="operationBindingObjectTransactional"></a>`transactional` | boolean | Marks this producer as transactional. When `true`, the Kafka client MUST be configured with a `transactional.id` at runtime. | OPTIONAL [`false`] — `send` only | MUST NOT be set on `receive` operations
<a name="operationBindingObjectIsolationLevel"></a>`isolationLevel` | string | Controls the visibility of transactional messages to this consumer. | OPTIONAL [`read_uncommitted`] — `receive` only | MUST be one of `read_uncommitted`, `read_committed`. SHOULD be `read_committed` when consuming from a transactional topic
<a name="operationBindingObjectErrorTopics"></a>`errorTopics` | [Error Topics Object](#errorTopics) | Configuration for retry and dead-letter queue (DLQ) topics associated with this consumer operation. Enables full Kafka infrastructure provisioning from a single AsyncAPI document without defining error topics as separate channels. | OPTIONAL — `receive` only | -
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed. | OPTIONAL [`latest`] | -

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  user-signedup:
operations:
  userSignup:
    action: receive
    bindings:
      kafka:
        groupId:
          type: string
          enum: ['myGroupId']
        clientId:
          type: string
          enum: ['myClientId']
        bindingVersion: '0.6.0'
```

```yaml
operations:
  publishPaymentEvent:
    action: send
    bindings:
      kafka:
        principal: 'User:payment-producer'
        transactional: true
        bindingVersion: '0.6.0'
```

```yaml
operations:
  consumePaymentEvents:
    action: receive
    bindings:
      kafka:
        groupId:
          type: string
          enum: ['payments-consumer-group']
        principal: 'User:payment-consumer'
        isolationLevel: read_committed
        errorTopics:
          addressTemplate: '${groupId}.__.${channel.address}.${suffix}'
          retryTopics: 3
          retry:
            partitions: 1
            replicas: 2
            topicConfiguration:
              retention.ms: 259200000
          dlq:
            partitions: 1
            replicas: 2
            topicConfiguration:
              retention.ms: 2592000000
              cleanup.policy: ["delete"]
        bindingVersion: '0.6.0'
```


<a name="errorTopics"></a>

## Error Topics Object

This object configures retry and dead-letter queue (DLQ) topics for a consumer operation.

##### Fixed Fields

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|:---:|:---:|---
<a name="errorTopicsAddressTemplate"></a>`addressTemplate` | string | Template for generated topic addresses. Supports variables: `${groupId}` (first enum value of `groupId`), `${channel.address}` (the channel's address), `${suffix}` (`retry-0` … `retry-{N-1}` and `dlq`). | REQUIRED | -
<a name="errorTopicsRetryTopics"></a>`retryTopics` | integer | Number of retry topics to create (named `retry-0` through `retry-{N-1}`). | OPTIONAL | MUST be a positive integer. REQUIRED when `retry` is present.
<a name="errorTopicsRetry"></a>`retry` | [Error Topic Configuration Object](#errorTopicConfiguration) \| [Reference Object](referenceObject) | Topic configuration applied to each retry topic. | OPTIONAL | -
<a name="errorTopicsDlq"></a>`dlq` | [Error Topic Configuration Object](#errorTopicConfiguration) \| [Reference Object](referenceObject) | Topic configuration applied to the DLQ topic. | OPTIONAL | -

<a name="errorTopicConfiguration"></a>
## Error Topic Configuration Object

This object defines the topic settings used by `retry` and `dlq`.

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|:---:|:---:|---
`partitions` | integer | Number of partitions for this topic. | OPTIONAL | MUST be positive
`replicas` | integer | Replication factor for this topic. | OPTIONAL | MUST be positive
`topicConfiguration` | [TopicConfiguration Object](#topicConfiguration) | Topic configuration properties. | OPTIONAL | -


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in Kafka.

##### Fixed Fields

Field Name |  Type   |                                                                             Description                                                                              | Applicability [default] | Constraints
---|:-------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------:|---
<a name="messageBindingObjectKey"></a>`key` | [Schema Object][schemaObject] \| [Reference Object](referenceObject) \| [AVRO Schema Object](https://avro.apache.org/docs/current/spec.html) | The message key. **NOTE**: You can also use the [reference object](referenceObject) way. | OPTIONAL | -
<a name="messageBindingObjectSchemaIdLocation"></a>`schemaIdLocation` | string | If a Schema Registry is used when performing this operation, tells where the id of schema is stored (e.g. `header` or `payload`). | OPTIONAL | MUST NOT be specified if `schemaRegistryUrl` is not specified at the Server level
<a name="messageBindingObjectSchemaIdPayloadEncoding"></a>`schemaIdPayloadEncoding` | string | Number of bytes or vendor specific values when schema id is encoded in payload (e.g `confluent`/ `apicurio-legacy` / `apicurio-new`). | OPTIONAL | MUST NOT be specified if `schemaRegistryUrl` is not specified at the Server level
<a name="messageBindingObjectSchemaLookupStrategy"></a>`schemaLookupStrategy` | string | Freeform string for any naming strategy class to use. Clients should default to the vendor default if not supplied. | OPTIONAL | MUST NOT be specified if `schemaRegistryUrl` is not specified at the Server level
<a name="messageBindingObjectCompatibility"></a>`compatibility` | string | The schema compatibility mode used when registering this schema with the schema registry. Values derive from compatibility types defined by Confluent Schema Registry and supported by Apicurio Registry and Karapace. | OPTIONAL | MUST be one of `BACKWARD`, `BACKWARD_TRANSITIVE`, `FORWARD`, `FORWARD_TRANSITIVE`, `FULL`, `FULL_TRANSITIVE`, `NONE`. MUST NOT be specified if `schemaRegistryUrl` is not specified at the Server level.
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed. | OPTIONAL [`latest`] | -

This object MUST contain only the properties defined above.

This example is valid for any Confluent compatible schema registry. Here we describe the implementation using the first 4 bytes in payload to store schema identifier.

```yaml
channels:
  test:
    address: test-topic
    messages:
      testMessage:
        bindings:
          kafka:
            key:
              type: string
              enum: ['myKey']
            schemaIdLocation: 'payload'
            schemaIdPayloadEncoding: '4'
            bindingVersion: '0.6.0'
```

This is another example that describes the use if Apicurio schema registry. We describe the `apicurio-new` way of serializing without details on how it's implemented. We reference a [specific lookup strategy](https://www.apicur.io/registry/docs/apicurio-registry/2.2.x/getting-started/assembly-using-kafka-client-serdes.html#registry-serdes-concepts-strategy_registry) that may be used to retrieve schema Id from registry during serialization.

```yaml
channels:
  test:
    address: test-topic
    messages:
      testMessage:
        bindings:
          kafka:
            key:
              type: string
              enum: ['myKey']
            schemaIdLocation: 'payload'
            schemaIdPayloadEncoding: 'apicurio-new'
            schemaLookupStrategy: 'TopicIdStrategy'
            bindingVersion: '0.6.0'
```

```yaml
messages:
  PaymentEvent:
    bindings:
      kafka:
        schemaIdLocation: 'payload'
        schemaLookupStrategy: 'TopicNameStrategy'
        compatibility: 'BACKWARD'
        bindingVersion: '0.6.0'
```

[schemaObject]: https://www.asyncapi.com/docs/reference/specification/v3.0.0-next-major-spec.15#schemaObject
[referenceObject]: https://www.asyncapi.com/docs/reference/specification/v3.0.0-next-major-spec.15#referenceObject
