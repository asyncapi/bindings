# Pulsar Bindings
This document defines how to describe Apache Pulsar specific information with AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.1.0`.

<a name="server"></a>
## Server Binding Object

This object contains information about the server representation in Pulsar.

##### Fixed Fields

Field Name | Type | Description | Applicability [default] |
---|:---:|:---|:---|
`tenant` | String | The pulsar tenant. If omitted, "public" must be assumed. | OPTIONAL [`public`] |
`bindingVersion` | String | The version of this binding. If omitted, "latest" MUST be assumed. | OPTIONAL [`latest`] |

##### Example

```yaml
servers:
  production:
    bindings:
      pulsar:
        tenant: contoso
        bindingVersion: '0.1.0'
```

<a name="channel"></a>
## Channel Binding Object
This object contains information about the channel representation in Pulsar

##### Fixed Fields

Field Name | Type | Description | Applicability [default] |
---|:---:|:---|:---|
`namespace` | String |  The namespace the channel is associated with. | REQUIRED |
`persistence` | String | Persistence of the topic in Pulsar. It MUST be either `persistent` or `non-persistent`. | REQUIRED |
`compaction`| Integer | Topic compaction threshold given in Megabytes. | OPTIONAL |
`geo-replication` | String[] | A list of clusters the topic is replicated to. | OPTIONAL |
`retention` | [Retention Definition Object](#retention-definition-object) | Topic retention policy.  | OPTIONAL |
`ttl` | Integer |  Message time-to-live in seconds. | OPTIONAL |
`deduplication` | Boolean | Message deduplication. When true, it ensures that each message produced on Pulsar topics is persisted to disk only once. | OPTIONAL |
`bindingVersion` | String | The version of this binding. If omitted, "latest" MUST be assumed. | OPTIONAL [`latest`] |

<a name="retention-definition-object"></a>
### Retention Definition Object
The `Retention Definition Object` is used to describe the Pulsar [Retention](https://pulsar.apache.org/docs/cookbooks-retention-expiry/) policy. If retention is specified, both fields are mandatory.

Field Name | Type | Description | Applicability [default] |
---|:---:|:---|:---|
`time`|Integer| Time given in Minutes. | OPTIONAL [`0`]
`size`|Integer| Size given in MegaBytes. | OPTIONAL [`0`]

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
        bindingVersion: '0.1.0'
```

<a name="operation"></a>
## Operation binding fields
This object MUST NOT contain any properties. Its name is reserved for future use.

<a name="message"></a>
## Message binding fields
This object MUST NOT contain any properties. Its name is reserved for future use.