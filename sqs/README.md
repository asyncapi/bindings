# SQS Bindings

This document defines how to describe SQS-specific information on AsyncAPI.

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

This object contains information about the operation representation in SQS.

##### Fixed Fields

Field Name | Type | Applies To | Description
---|:---:|:---:|---
<a name="operationBindingObjectVersion"></a>`verison` | String | Publish, Subscribe | Defines binding verison.
<a name="operationBindingObjectIs"></a>`is` | String | Publish, Subscribe | Defines if queue has been configured as standard or FIFO. Its value MUST be either fifo or standard.
<a name="operationBindingObjectFifo"></a>`fifo` | string | Publish, Subscribe | When is = fifo, defines config for first in first out queue.
<a name="operationBindingObjectName"></a>`fifo.name` | string | Publish, Subscribe | Name of queue, must end in .fifo suffix for FIFO queues.
<a name="operationBindingObjectOrderBy"></a>`fifo.orderBy` | string | Publish, Subscribe | Tag used to define what message group a message belongs too, messages of the same group are always processed in order.
<a name="operationBindingObjectDeDupeBy"></a>`fifo.deDupeBy` | string | Publish, Subscribe | Tag used to define what message group a message belongs too, messages of the same group are always processed in order.
<a name="operationBindingObjectStandard"></a>`standard` | string | Publish, Subscribe | When is = standard, high performance best efforts ordering.
<a name="operationBindingObjectName"></a>`standard.name` | string | Publish, Subscribe | Name of queue, no suffix allowed.

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  user/signedup:
    publish:
      bindings:
        sqs:
          version: 0.2.0
          is: fifo
          fifo:
            name: myQueueName.fifo
            orderBy: messageGroupId
            deDupeBy: messageDuplicationId
#
#         When a standard high performanace queue. May not delier in order (non-FIFO)
#
#         is: standard
#         standard:
#           name: myQueueName


```

<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.
