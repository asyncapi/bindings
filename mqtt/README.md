# MQTT Bindings

This document defines how to describe MQTT-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.1.0`.


<a name="server"></a>

## Server Binding Object

This object contains information about the server representation in MQTT.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="serverBindingObjectClientId"></a>`clientId` | string | The client identifier.
<a name="serverBindingObjectCleanSession"></a>`cleanSession` | boolean | Whether to create a persistent connection or not. When `false`, the connection will be persistent.
<a name="serverBindingObjectLastWill"></a>`lastWill` | object | Last Will and Testament configuration.
<a name="serverBindingObjectLastWillTopic"></a>`lastWill.topic` | string | The topic where the Last Will and Testament message will be sent.
<a name="serverBindingObjectLastWillQoS"></a>`lastWill.qos` | integer | Defines how hard the broker/client will try to ensure that the Last Will and Testament message is received. Its value MUST be either 0, 1 or 2.
<a name="serverBindingObjectLastWillMessage"></a>`lastWill.message` | string | Last Will message.
<a name="serverBindingObjectLastWillRetain"></a>`lastWill.retain` | boolean | Whether the broker should retain the Last Will and Testament message or not.
<a name="serverBindingObjectKeepAlive"></a>`keepAlive` | integer | Interval in seconds of the longest period of time the broker and the client can endure without sending a message.
<a name="serverBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
servers:
  production:
    bindings:
      mqtt:
        clientId: guest
        cleanSession: true
        lastWill:
          topic: /last-wills
          qos: 2
          message: Guest gone offline.
          retain: false
        keepAlive: 60
        bindingVersion: 0.1.0
```


<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.



<a name="operation"></a>

## Operation Binding Object

This object contains information about the operation representation in MQTT.

##### Fixed Fields

Field Name | Type | Applies To | Description
---|:---:|:---:|---
<a name="operationBindingObjectQoS"></a>`qos` | integer | Publish, Subscribe | Defines the Quality of Service (QoS) levels for the message flow between client and server. Its value MUST be either 0 (At most once delivery), 1 (At least once delivery), or 2 (Exactly once delivery).
<a name="operationBindingObjectRetain"></a>`retain` | boolean | Publish, Subscribe | Whether the broker should retain the message or not.
<a name="operationBindingObjectBindingVersion"></a>`bindingVersion` | string | Publish, Subscribe | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
channels:
  user/signup:
    publish:
      bindings:
        mqtt:
          qos: 2
          retain: true
          bindingVersion: 0.1.0
```


<a name="message"></a>

## Message Binding Object

This object contains information about the message representation in MQTT.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="messageBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

```yaml
channels:
  user/signup:
    publish:
      message:
        bindings:
          mqtt:
            bindingVersion: 0.1.0
```
