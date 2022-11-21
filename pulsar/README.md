# Pulsar Bindings
This document defines how to describe Apache Pulsar specific information with AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.1.0`.

<a name="server"></a>
## Server Binding Object

This object contains information about the server representation in Pulsar.

##### Fixed Fields

Field Name | Type | Description |
---|:---:|:---:|
`tenant` | String | **Optional**. The pulsar tenant. If omitted, "public" must be assumed. |
`bindingVersion` | String | **Required**. The version of this binding. If omitted, "latest" MUST be assumed. |

This object MUST contain only the properties defined above.

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

Field Name | Type | Description |
---|:---:|:---:|
`namespace` | String | **Optional**. The namespace, the channel is associated with. If omitted, "public" MUST be assumed. |
`persistence` | String | **Required**. persistence of the topic in Pulsar `persistent` or `non-persistent`. |
`compaction`| Integer | **Optional**. Topic compaction threshold given in Megabytes. |
`geo-replication` | String[] | **Optional**. A list of clusters the topic is replicated to. |
`retention` | **Optional**. [Retention Definition Object](#retention-definition-object) | Topic retention policy.  |
`ttl` | Integer | **Optional**. Message Time-to-live in seconds. |
`deduplication` | Boolean | **Optional**. When Message deduplication is enabled, it ensures that each message produced on Pulsar topics is persisted to disk only once. |
`bindingVersion` | String | **Required**. The version of this binding. If omitted, "latest" MUST be assumed. |

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  user-signedup:
    bindings:
      pulsar:
        namespace: 'staging'
        persistence: 'persistent'
        compaction: 1000
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

<a name="retention-definition-object"></a>
## Retention Definition Object
The `Retention Definition Object` is used to describe the Pulsar [Retention](https://pulsar.apache.org/docs/cookbooks-retention-expiry/) policy. If retention is specified, both fields are mandatory.

Field Name | Type | Description
---|---|---
`time`|Integer| **Required**. Time given in Minutes. `0` = Disable message retention (by default)|
`size`|Integer| **Required**. Size given in MegaBytes. `0` = Disable message retention (by default)|
