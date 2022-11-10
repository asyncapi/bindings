# Pulsar Bindings
This document defines how to describe Apache Pulsar specific information with AsyncAPI.

## Server Binding Object
##### Fixed Fields

Field Name | Type | Description |
---|:---:|:---:|
`retention` | [Retention Definition Object](#retention-definition-object) | Topic retention policy  |
`deduplication` | Boolean | When Message deduplication is enabled, it ensures that each message produced on Pulsar topics is persisted to disk only once |

This object MUST contain only the properties defined above.

## Channel Binding Object
This object contains information about the channel representation in Pulsar

##### Fixed Fields

Field Name | Type | Description |
---|:---:|:---:|
`persistence` | String | persistence of the topic in Pulsar `persistent` or `non-persistent` |
`compaction`| Integer64 | Topic compaction threshold given in bytes |
`retention` | [Retention Definition Object](#retention-definition-object) | Topic retention policy  |
`deduplication` | Boolean | When Message deduplication is enabled, it ensures that each message produced on Pulsar topics is persisted to disk only once |

This object MUST contain only the properties defined above.

## Potential Operation binding fields
This object MUST NOT contain any properties. Its name is reserved for future use.
## Potential Message binding fields
This object MUST NOT contain any properties. Its name is reserved for future use.

## Retention Definition Object
The `Retention Definition Object` is used to describe the Pulsar [Retention](https://pulsar.apache.org/docs/cookbooks-retention-expiry/) policy 

Field Name | Type | Description
---|---|---
`time`|Integer| Time given in Minutes. `0` = Disable message retention (by default)|
`size`|Integer| Size given in MegaBytes. `0` = Disable message retention (by default)|