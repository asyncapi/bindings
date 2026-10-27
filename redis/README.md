# Redis Bindings

This document defines how to describe Redis-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.2.0`.


<a name="server"></a>

## Server Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.



<a name="channel"></a>

## Channel Binding Object

This object contains information about the channel representation in Redis. The channel `address` is the Redis key of the stream or the name of the Pub/Sub channel.

##### Fixed Fields

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|---|:---:|---
<a name="channelBindingObjectType"></a>`type` | string | The Redis messaging primitive used by this channel: `stream` ([Redis Streams](https://redis.io/docs/latest/develop/data-types/streams/)) or `pubsub` ([Redis Pub/Sub](https://redis.io/docs/latest/develop/interact/pubsub/)). | REQUIRED | MUST be `stream` or `pubsub`
<a name="channelBindingObjectMaxLen"></a>`maxLen` | integer | Maximum number of entries kept in the stream. Producers MUST trim the stream exactly to this length when adding entries (`XADD <key> MAXLEN <maxLen> ...`). | OPTIONAL | MUST be greater than or equal to `1`. MUST NOT be present when `type` is `pubsub`
<a name="channelBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed. | OPTIONAL [`latest`] | -

This object MUST contain only the properties defined above.

##### Examples

```yaml
channels:
  events:
    address: 'app:events'
    bindings:
      redis:
        type: stream
        maxLen: 1
        bindingVersion: '0.2.0'
  notifications:
    address: 'app:notifications'
    bindings:
      redis:
        type: pubsub
        bindingVersion: '0.2.0'
```



<a name="operation"></a>

## Operation Binding Object

This object contains information about the operation representation in Redis.

##### Fixed Fields

Field Name | Type | Description | Applicability [default] | Constraints
---|:---:|---|:---:|---
<a name="operationBindingObjectConsumerGroup"></a>`consumerGroup` | string | Name of the [consumer group](https://redis.io/docs/latest/develop/data-types/streams/#consumer-groups) used to read the stream (`XREADGROUP GROUP <consumerGroup> <consumer> ...`). Consumers of the same group share the entries: each new entry is delivered to one consumer of the group, and unacknowledged entries can later be claimed by another consumer (`XCLAIM`, `XAUTOCLAIM`); different groups each receive every entry. If omitted, the stream is read without a consumer group (`XREAD`). | OPTIONAL | MUST only be used in operations with `action: receive` on a channel with `type: stream`
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed. | OPTIONAL [`latest`] | -

The consumer name inside the group is chosen by each running instance and is not part of the API description.

This object MUST contain only the properties defined above.

##### Examples

```yaml
operations:
  consumeEvent:
    action: receive
    channel:
      $ref: '#/channels/events'
    bindings:
      redis:
        consumerGroup: my-service
        bindingVersion: '0.2.0'
```



<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.
