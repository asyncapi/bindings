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
<a name="operationBindingObjectAttributeName"></a>`fifo.attributeNames` | list | Subscribe | Attributes to be returned. Includes All, ApproximateFirstReceiveTimestamp, ApproximateReceiveCount, AWSTraceHeader, SenderId, SentTimestamp, MessageDeduplicationId, MessageGroupId, SequenceNumber
<a name="operationBindingObjectmessageAttributeName"></a>`fifo.messageAttributeName` | string | Publish, Subscribe | Custom attribute messages.
<a name="operationBindingObjectOrderBy"></a>`fifo.orderBy` | string | Publish | Tag used to define what message group a message belongs too, messages of the same group are always processed in order.
<a name="operationBindingObjectDeDupeBy"></a>`fifo.deDupeBy` | string | Publish | Tag used to define what message group a message belongs too, messages of the same group are always processed in order.
<a name="operationBindingObjectAttemptId"></a>`fifo.receiveRequestAttemptId` | string | Subscribe | For FIFO queues, the deduplication token used for subscribe message calls.
<a name="operationBindingObjectMaxMessages"></a>`fifo.maxNumberOfMessages` | integer | Subscribe | Max number of messages to return.
<a name="operationBindingObjectVisTimeOut"></a>`fifo.visibilityTimeout` | integer | Subscribe | Duration in seconds messages are hidden from subsequent retrieve attempts.
<a name="operationBindingObjectWaitTime"></a>`fifo.waitTimeSeconds` | integer | Subscribe | Duration in seconds for call to wait before returning if no messages are available.
<a name="operationBindingObjectAction"></a>`fifo.action` | String | Publish, Subscribe | 'SendMessage' for Publish. 'ReceiveMessage' for Subscribe.
<a name="operationBindingObjectStandard"></a>`standard` | string | Publish, Subscribe | When is = standard, high performance best efforts ordering.
<a name="operationBindingObjectName"></a>`standard.name` | string | Publish, Subscribe | Name of queue, no suffix allowed.
<a name="operationBindingObjectAttributeName"></a>`standard.attributeNames` | list | Subscribe | Attributes to be returned. Includes All, ApproximateFirstReceiveTimestamp, ApproximateReceiveCount, AWSTraceHeader, SenderId, SentTimestamp, MessageDeduplicationId, MessageGroupId, SequenceNumber
<a name="operationBindingObjectmessageAttributeName"></a>`standard.messageAttributeName` | string | Publish, Subscribe | Custom attribute messages.
<a name="operationBindingObjectMaxMessages"></a>`standard.maxNumberOfMessages` | integer | Subscribe | Max number of messages to return.
<a name="operationBindingObjectVisTimeOut"></a>`standard.visibilityTimeout` | integer | Subscribe | Duration in seconds messages are hidden from subsequent retrieve attempts.
<a name="operationBindingObjectWaitTime"></a>`standard.waitTimeSeconds` | integer | Subscribe | Duration in seconds for call to wait before returning if no messages are available.
<a name="operationBindingObjectAction"></a>`standard.action` | String | Publish, Subscribe | 'SendMessage' for Publish. 'ReceiveMessage' for Subscribe.

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
user/signedup:
  publish:
    bindings:
      sqs:
        version: '0.2.0'
        is: fifo
        fifo:
          name: https://sqs.ap-soautheast-2.amazonaws.com/75555555/msqs-test.fifo
          orderBy: messageGroupId
          dedupeBy: messageDuplicationId
        action: SendMessage
#
#         When a standard high performanace queue. May not deliver in order (non-FIFO)
#
#         is: standard
#         standard:
#           name: myQueueName
#         action: SendMessage

    summary: Publishing test events
    operationId: pushTestMessage
    message:
      payload:
        type: object
        properties:
          id:
            type: int
            minimum: 0
            description: id of message
          messageBody:
            type: string
            description: The body of the message

  subscribe:
    bindings:
      sqs:
        vesion: 0.2.0
        is: fifo
        fifo:
          name: https://sqs.ap-soautheast-2.amazonaws.com/75555555/msqs-test.fifo
          attributeNames: 
            - All
          messageAttributeName:
            - myCustomAttribute1
            - myCustomAttribute2
          maxNumberOfMessages: 10
          visibilityTimeout: 20
          waitTimeSeconds: 2
          receiveRequestAttemptId: attempt123
          action: RecieveMessage
            

```

<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.
