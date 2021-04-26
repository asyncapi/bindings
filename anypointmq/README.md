# Anypoint MQ Bindings

This document defines how to describe Anypoint MQ-specific information in AsyncAPI documents. 

[Anypoint MQ](https://docs.mulesoft.com/mq/) is MuleSoft's multi-tenant, cloud messaging service and is fully integrated with [Anypoint Platform](https://www.mulesoft.com/platform/enterprise-integration).

<a name="version"></a>
## Version

The version of this bindings specification is `0.0.1`.
This is also the `bindingVersion` for all binding objects defined by this specification.
In any given binding object, `latest` can alternatively be used as long as this version of the bindings specification remains the latest published version.

The version of the AsyncAPI specification to which these bindings apply is `2.0.0`.

### Backwards Compatibility

All bindings defined in this specification allow additional, unspecified fields, so that a concrete instance of a binding object MAY contain arbitrary fields not defined in this specification. This is to ensure backwards compatibility of binding objects, and also to allow code generators to define their own, specific fields which are not defined in this bindings specification.

As an example of backwards compatibility, consider a concrete binding object valid against version `1.1.0` of this specification. This binding object may specify `bindingVersion: 1.1.0`. This binding object may contain fields specific to version `1.1.0`, which were not yet defined in, say, version `1.0.5` of this specification. However, because additional fields are allowed, this binding object is also valid against version `1.0.5` of this specification. The binding object valid against version `1.1.0` can therefore be used in a system that is only aware of version `1.0.5`, although all fields not yet defined in version `1.0.5` of this specification will be ignored.

## Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this bindings specification are to be interpreted as described in IETF [RFC2119](https://www.ietf.org/rfc/rfc2119.txt).

## Protocol

These bindings use the `anypointmq` [protocol](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#definitionsProtocol) in AsyncAPI documents to denote connections to and interactions with Anypoint MQ message brokers.

The Anypoint MQ protocol is based on invocations of the [Anypoint MQ Broker REST API](https://docs.mulesoft.com/mq/mq-apis#mqbrokerapi).

## Code Generation

An AsyncAPI document defines a machine-readable contract for the message-driven API exposed by an application. The application promises to adhere to that contract, and its "interaction partners" can send and/or receive messages accordingly.

Code generation based on an AsyncAPI document can be used to

- generate a skeleton of the application that exposes the message-driven API (this is common), or
- generate a stub of an "interaction partner" application that interacts with that application via its message-driven API (this is less common).

Some of the fields in the bindings defined in this specification serve to more clearly define the contract of an API in the presence of an Anypoint MQ message broker, while other fields are useful primarily for code generation. The latter fields can be used in both code generation scenarios, and will then influence the application code that is being generated.

## Server Object

The fields of the standard [Server Object](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#serverObject) are constrained and interpreted as follows:

- `protocol` is *required* and MUST be `anypointmq` for the scope of this specification.
- `url` is *required* and MUST be the endpoint URL of the Anypoint MQ Broker REST API _excluding_ the final major version indicator (e.g., `v1`), such as `https://mq-us-east-1.anypoint.mulesoft.com/api` or `https://mq-eu-central-1.eu1.anypoint.mulesoft.com/api` (and _not_ `https://.../api/v1`). MUST NOT use a scheme other than `https` or `http` (as supported by the Broker REST API endpoint).
- `protocolVersion` is *optional* and if present MUST be the major version indicator of the Anypoint MQ Broker REST API omitted from the `url`, e.g. `v1`. Defaults to `v1` if absent.

TODO:
- authentication is by Anypoint MQ client ID and secret, which implies the Anypoint Platform organization ID and environment (ID)
- capture this as security scheme and Security Requirement Object

<a name="server"></a>
## Server Binding Object

The Anypoint MQ [Server Binding Object](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#serverBindingsObject) is defined by a [JSON Schema](json_schemas/server.json), which defines these *optional* fields:

- `proxy.host`: Defines use of a HTTP proxy for interactions with the Anypoint MQ broker: Destination host for proxy requests.
- `proxy.port`: Defines use of a HTTP proxy for interactions with the Anypoint MQ broker: Destination port for proxy requests.
- `proxy.username`: Defines use of a HTTP proxy for interactions with the Anypoint MQ broker: Username to authenticate against the proxy.
- `proxy.password`: Defines use of a HTTP proxy for interactions with the Anypoint MQ broker: Password to authenticate against the proxy.
- `sendBufferSize`: Size of the buffer (in bytes) used when sending data, set on the socket itself.
- `receiveBufferSize`: Size of the buffer (in bytes) used when receiving data.
- `clientTimeout`: SO_TIMEOUT value on sockets. Indicates the amount of time (in milliseconds) that the socket waits in a blocking operation before failing. A value of 0 indicates an indefinite wait.
- `sendTCPWithNoDelay`: If set, transmitted data is not grouped but sent immediately.
- `linger`: SO_LINGER value, which determines how long (in milliseconds) the socket takes to close so that any remaining data is transmitted correctly.
- `keepAlive`: SO_KEEPALIVE behavior on open sockets, which automatically checks open socket connections that are unused for long periods, and closes them if the connection becomes unavailable. This is a property on the socket itself and is used by a server socket to control whether connections to the server are kept alive before they are recycled.
- `connectionTimeout`: Number of milliseconds to wait until an outbound connection to a remote server is successfully created, before failing with a timeout.

Additional fields MAY be present but are ignored if the server binding object is interpreted according to this version of the bindings specification.

Note that all of these fields serve _code generation_ based on the AsyncAPI document, rather than defining the contract of the message-driven API.

### Examples

The following example shows a `servers` object with two servers, both using `anypointmq` as the `protocol`, and one having a server binding object for `anypointmq`:

```yaml
servers:
  development:
    url:         https://mq-us-east-1.anypoint.mulesoft.com/api
    description: |
      Anypoint MQ broker for development, in the US East (N. Virginia) runtime plane under management of the US control plane.
      Minimal configuration, using defaults for all fields of the server binding object.
      Implicitly uses v1 of the anypointmq protocol and hence the Anypoint MQ Broker REST API.
    protocol: anypointmq

  production:
    url:         https://mq-eu-central-1.eu1.anypoint.mulesoft.com/api
    description: |
      Anypoint MQ broker for production, in the EU Central (Frankfurt) runtime plane under management of the EU control plane.
      Complete configuration, providing explicit values for all fields of the server binding object.
      Explicitly specifies the use of v1 of the anypointmq protocol and hence the Anypoint MQ Broker REST API.
    protocol:        anypointmq
    protocolVersion: v1
    bindings:
      anypointmq:
        bindingVersion: 0.0.1
        proxy:
          host:    proxy.corporate.com
          port:    80
          username: proxy-user
          password: passwd-of-proxy-user
        sendBufferSize:     1024
        receiveBufferSize:  2048
        clientTimeout:      1000
        sendTCPWithNoDelay: true
        linger:             1000
        keepAlive:          true
        connectionTimeout:  10000
```

<a name="channel"></a>
## Channel Binding Object

The Anypoint MQ [Channel Binding Object](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#channel-bindings-object) is defined by a [JSON Schema](json_schemas/channel.json) that currently defines *no fields*.

Fields MAY be present but are ignored if the channel binding object is interpreted according to this version of the bindings specification.

Because an empty channel binding object does not add any information to an AsyncAPI document, it is RECOMMENDED that AsyncAPI documents omit channel binding objects for `anypointmq`.

### Examples

The following example shows a `channels` object with two channels, one having a semantically empty channel binding object for `anypointmq` (which is not recommended):

```yaml
channels:
  user/signedup:
    description: |
      This application sends events to this channel about users that have signed up.
      Minimal and recommended configuration, not specifying an empty channel binding object.
    subscribe:
      //...
  user/signup:
    description: |
      This application receives command messages from this channel about users to sign up.
      Non-recommended configuration, explicitly providing a semantically empty channel binding object.
    bindings:
      anypointmq:
      	bindingVersion: 0.0.1
    publish:
      //...
```

<a name="operation"></a>
## Operation Binding Object

The Anypoint MQ [Operation Binding Object](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#operation-bindings-object) is defined by a [JSON Schema](json_schemas/operation.json), which defines these *optional* fields:

TODO

Additional fields MAY be present but are ignored if the operation binding object is interpreted according to this version of the bindings specification.

### Examples

The following example shows a `channels` object with two channels, each having one operation (`subscribe` or `publish`). Only the `publish` operation has an operation binding object for `anypointmq`:

```yaml
channels:
  user/signedup:
    subscribe:
      operationId: userHasSignedUp
      description: |
        TODO
      bindings:
        anypointmq:
          ack: TODO
      message:
        //...
  user/signup:
    publish:
      operationId: signUpUser
      description: |
        TODO
      bindings:
        anypointmq:
          bindingVersion: 0.0.1
          // Destination (Queue or Exchange) name for this channel. Defaults to the channel name. SHOULD only be specified if the channel name differs from the actual destination name, such as when the channel name is not a valid destination name in Anypoint MQ.
          destination:             user-signup-queue
          consumer:
            // or manual | default immediate | Acknowledgment mode to use for the messages retrieved
            acknowledgmentMode:    immediate
            // Duration in milliseconds that a message is held by a consumer waiting for an acknowledgment or not acknowledgment. After that duration elapses, the message is again available to any consumer.
            acknowledgmentTimeout: 10000
            // default 10000 | Time in milliseconds to wait for a message to be ready for consumption
            pollingTime:           10000
          producer:
      message:
        //...
```

<a name="message"></a>
## Message Binding Object

The Anypoint MQ [Message Binding Object](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#message-bindings-object) is defined by a [JSON Schema](json_schemas/message.json), which defines these *optional* fields:

TODO

Additional fields MAY be present but are ignored if the message binding object is interpreted according to this version of the bindings specification.

### Examples

```yaml
TODO
```
