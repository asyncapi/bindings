# Pulsar Bindings
This document defines how to describe Apache Pulsar specific information with AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.2.0`.

<a name="server"></a>
## Server Binding Object

This object contains information about the server representation in Pulsar.

##### Fixed Fields

Field Name | Type | Required | Description | Default value |
---|:---:|:---:|:---|:---|
`tenant` | String | No | The pulsar tenant. If omitted, "public" MUST be assumed. | `public` |
`bindingVersion` | String | No | The version of this binding. If omitted, "latest" MUST be assumed. | `latest` |

##### Example

```yaml
servers:
  production:
    bindings:
      pulsar:
        tenant: contoso
        bindingVersion: '0.2.0'
```

<a name="channel"></a>
## Channel Binding Object
This object contains information about the channel representation in Pulsar

##### Fixed Fields

Field Name | Type | Required | Description | Default value |
---|:---:|:---:|:---|:---|
`namespace` | String | Yes |  The namespace the channel is associated with. | N/A |
`persistence` | String | Yes | Persistence of the topic in Pulsar. It MUST be either `persistent` or `non-persistent`. | N/A |
`compaction`| Integer | No | Topic compaction threshold given in Megabytes. | N/A |
`geo-replication` | String[] | No | A list of clusters the topic is replicated to. | N/A |
`retention` | [Retention Definition Object](#retention-definition-object) | No | Topic retention policy.  | N/A |
`ttl` | Integer | No |  Message time-to-live in seconds. | N/A |
`deduplication` | Boolean | No | Message deduplication. When true, it ensures that each message produced on Pulsar topics is persisted to disk only once. | N/A |
`bindingVersion` | String | No | The version of this binding. If omitted, "latest" MUST be assumed. | `latest` |

<a name="retention-definition-object"></a>
### Retention Definition Object
The `Retention Definition Object` is used to describe the Pulsar [Retention](https://pulsar.apache.org/docs/cookbooks-retention-expiry/) policy.

Field Name | Type | Required | Description | Default value |
---|:---:|:---:|:---|:---|
`time`|Integer| No | Time given in Minutes. | `0` |
`size`|Integer| No |Size given in MegaBytes. | `0` |

##### Example

```yaml
channels:
  user-signedup:
    bindings:
      pulsar:
        namespace: 'staging'
        persistence: 'persistent'
        compaction: 1000
        geo-replication:
          - 'us-east1'
          - 'us-west1'
        retention:
          time: 7
          size: 1000
        ttl: 360
        deduplication: false
        bindingVersion: '0.2.0'
```

<a name="operation"></a>
## Operation binding fields
This object MUST NOT contain any properties. Its name is reserved for future use.

<a name="message"></a>
## Message Binding Object
This object contains information about the message representation in Pulsar.

##### Fixed Fields

Field Name | Type | Required | Description | Default value |
---|:---:|:---:|:---|:---|
`key` | String | No | The message key, also called the partition key. It determines the partition on a partitioned topic, the [order](https://pulsar.apache.org/docs/next/concepts-messaging/#ordering-guarantee) for consumers using a [Key_Shared](https://pulsar.apache.org/docs/next/concepts-messaging/#key_shared) subscription, and which messages are kept by [topic compaction](https://pulsar.apache.org/docs/next/concepts-topic-compaction/). | N/A |
`orderingKey` | String | No | The ordering key. If present, it determines the order for consumers using a Key_Shared subscription, instead of `key`. | N/A |
`bindingVersion` | String | No | The version of this binding. If omitted, "latest" MUST be assumed. | `latest` |

##### Example

```yaml
channels:
  user-signedup:
    messages:
      userSignedUp:
        bindings:
          pulsar:
            key:
              type: string
              format: uuid
              description: The user ID. All events for the same user are delivered to the same partition.
            orderingKey:
              type: string,
              minLength: 3,
              maxLength: 64,
              format: email
              description: The account e-mail address. All events regarding the same account e-mail address are delivered in order.
            bindingVersion: '0.2.0'
```
