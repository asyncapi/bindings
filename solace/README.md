# Solace Bindings

This document defines how to describe Solace-specific information with AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.3.0`.

<a name="server"></a>

## Server Binding Object

Field Name | Type | Description
---|---|---
`bindingVersion`|String|The current version is 0.3.0
`msgVpn`|String|The Virtual Private Network name on the Solace broker.


<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.



<a name="operation"></a>

## Operation Binding Object

We need the ability to support several bindings for each operation, see the [Example](#example) section below for details.

Field Name | Type | Description
---|---|---
`bindingVersion`|String|The current version is 0.3.0
`destinations`|List of Destination Objects|Destination Objects are described next.

### Destination Object

Each destination has the following structure:

| Field Name                 | Type           | Description                                                                                                                                                                                                                          |
| -------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `destinationType`          | Enum           | 'queue' or 'topic'. If the type is queue, then the subscriber can bind to the queue, which in turn will subscribe to the topic as represented by the channel name or to the provided topicSubscriptions.                             |
| `deliveryMode`             | Enum           | 'direct' or 'persistent'. This determines the quality of service for publishing messages as documented [here.](https://docs.solace.com/Get-Started/Core-Concepts-Message-Delivery-Modes.htm) Default is 'persistent'.                |
| `queue.name`               | String         | The name of the queue, only applicable when destinationType is 'queue'.                                                                                                                                                              |
| `queue.topicSubscriptions` | List of String | A list of topics that the queue subscribes to, only applicable when destinationType is 'queue'. If none is given, the queue subscribes to the topic as represented by the channel name.                                              |
| `queue.accessType`         | Enum           | 'exclusive' or 'nonexclusive'. This is documented [here.](https://docs.solace.com/Messaging/Guaranteed-Msg/Endpoints.htm#Queues) Only applicable when destinationType is 'queue'.                                                    |
| `queue.maxMsgSpoolSize`    | String         | The maximum amount of message spool that the given queue may use. This is documented [here.](https://docs.solace.com/Messaging/Guaranteed-Msg/Message-Spooling.htm#max-spool-usage) Only applicable when destinationType is 'queue'. |
| `queue.maxTtl`             | String         | The maximum TTL to apply to messages to be spooled. This is documented [here.](https://docs.solace.com/Messaging/Guaranteed-Msg/Configuring-Queues.htm) Only applicable when destinationType is 'queue'.                             |
| `topic.topicSubscriptions` | List of String | A list of topics that the client subscribes to, only applicable when destinationType is 'topic'. If none is given, the client subscribes to the topic as represented by the channel name.                                            |

<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.



<a name="example"></a>

## Example with two destinations ##

Here is an example of when we could need two Solace destinations.

Imagine a system where there is a schema called Person, and there are topics:

`person/{personId}/created`

and

`person/{personId}/updated`

and you have one application that receives both events. We also want each to be on its own queue. The AsyncAPI file could look like this:

```yaml
components:
  schemas:
    Person:
      type: string        
  messages:
    PersonEvent:
      payload:
        $ref: '#/components/schemas/Person'
      schemaFormat: application/vnd.aai.asyncapi+json;version=2.0.0
      contentType: application/json
channels:
  'person/{personId}/{eventType}':
    publish:
      bindings:
        solace:
          bindingVersion: 0.3.0
          destinations:
            - destinationType: queue
              queue:
                name: CreatedHREvents
                topicSubscriptions:
                - person/*/created
            - destinationType: queue
              queue:
                name: UpdatedHREvents
                topicSubscriptions:
                - person/*/updated
      message:
        $ref: '#/components/messages/PersonEvent'
    parameters:
      personId:
        schema:
          type: string
      eventType:
        schema:
          type: string
asyncapi: 2.4.0
info:
  title: HRApp
  version: 0.0.1
```

The expected behaviour would be that the application binds to both queues, and each queue has its own topic subscription, one to created and one to updated events.


## Example with a wildcard subscription ##

This example shows how a client could receive all the topics under `person/` using a wildcard subscription:

```yaml
components:
  schemas:
    Person:
      type: string        
  messages:
    PersonEvent:
      payload:
        $ref: '#/components/schemas/Person'
      schemaFormat: application/vnd.aai.asyncapi+json;version=2.0.0
      contentType: application/json
channels:
  'person/{personId}/{eventType}':
    publish:
      bindings:
        solace:
          bindingVersion: 0.3.0
          destinations:
            - destinationType: topic
              topicSubscriptions:
              - person/>
      message:
        $ref: '#/components/messages/PersonEvent'
    parameters:
      personId:
        schema:
          type: string
      eventType:
        schema:
          type: string
asyncapi: 2.4.0
info:
  title: HRApp
  version: 0.0.1
```
