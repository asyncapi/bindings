# Anypoint MQ Bindings

This document defines how to describe Anypoint MQ-specific information in AsyncAPI documents. 

[Anypoint MQ](https://docs.mulesoft.com/mq/) is MuleSoft's multi-tenant, cloud messaging service and is fully integrated with [Anypoint Platform](https://www.mulesoft.com/platform/enterprise-integration).

<a name="version"></a>
## Version

Current version is `0.0.1`.

## Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this bindings specification are to be interpreted as described in IETF [RFC2119](https://www.ietf.org/rfc/rfc2119.txt).

## Protocol

These bindings use the `anypointmq` protocol in AsyncAPI documents to denote connections to Anypoint MQ message brokers.

The Anypoint MQ protocol is based on invocations of the [Anypoint MQ Broker REST API](https://docs.mulesoft.com/mq/mq-apis#mqbrokerapi).

## Server Object

The fields of the standard [Server Object](https://github.com/asyncapi/asyncapi/blob/master/versions/2.0.0/asyncapi.md#serverObject) are constrained and interpreted as follows:

- `protocol` is *required* and MUST be `anypointmq` for the scope of this specification.
- `url` is *required* and MUST be the endpoint URL of the Anypoint MQ Broker REST API _excluding_ the final major version indicator (e.g., `v1`), e.g., `https://mq-${REGION_ID}.anypoint.mulesoft.com/api`. MUST NOT use a scheme other than `https` or `http` (as supported by the Broker REST API endpoint).
- `protocolVersion` is *optional* and if present MUST be the major version indicator of the Anypoint MQ Broker REST API omitted from the `url`, e.g. `v1`. Defaults to `v1` if absent.

TODO:
- authentication is by Anypoint MQ client ID and secret, which implies the Anypoint Platform organization ID and environment (ID)
- capture this as security scheme and Security Requirement Object

<a name="server"></a>
## Server Binding Object

TODO

### Example

```yaml
servers:
  development:
    url: https://mq-us-east-1.anypoint.mulesoft.com/api
    description: Anypoint MQ broker for development, in the US East (N. Virginia) runtime plane under management of the US control plane
    protocol: anypointmq
    protocolVersion: v1

  production:
    url: https://mq-eu-central-1.eu1.anypoint.mulesoft.com/api
    description: Anypoint MQ broker for production, in the EU Central (Frankfurt) runtime plane under management of the EU control plane
    protocol: anypointmq
    protocolVersion: v1
    bindings:
      anypointmq:
        bindingVersion: 0.0.1
        proxy:
          host: proxy.corporate.com
          port: 80
          username: proxy-user
          password: passwd-of-proxy-user
        sendBufferSize: 1024
        receiveBufferSize: 2048
        clientTimeout: 1000
        sendTCPWithNoDelay: true
        linger: 1000
        keepAlive: true
        connectionTimeout: 30000
```

<a name="channel"></a>
## Channel Binding Object

TODO

<a name="operation"></a>
## Operation Binding Object

TODO

<a name="message"></a>
## Message Binding Object

TODO