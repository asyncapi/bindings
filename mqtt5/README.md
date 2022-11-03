# MQTT 5 Bindings

This document defines how to describe MQTT 5-specific information on AsyncAPI.

<a name="version"></a>

## Version

Current version is `0.2.0`.


<a name="server"></a>

## Server Binding Object

This object contains information about the server representation in MQTT5.

##### Fixed Fields

Field Name | Type | Description
---|:---:|---
<a name="serverBindingObjectSessionExpiryInterval"></a>`sessionExpiryInterval` | [Schema Object][schemaObject] \| integer | Session Expiry Interval in seconds.
<a name="serverBindingObjectBindingVersion"></a>`bindingVersion` | string | The version of this binding. If omitted, "latest" MUST be assumed.

This object MUST contain only the properties defined above.

##### Example

```yaml
servers:
  production:
    bindings:
      mqtt5:
        sessionExpiryInterval: 60
        bindingVersion: 0.2.0
```

<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.



<a name="operation"></a>

## Operation Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.




<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.

[schemaObject]: https://www.asyncapi.com/docs/specifications/2.4.0/#schemaObject
