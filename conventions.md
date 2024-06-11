# Conventions
## What is this?
This document describes a set of conventions that implementers may adopt when describing [Channel](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#channelBindingsObject), [Operation](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#operationBindingsObject) or [Message](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#messageBindingsObject) bindings for different protocols.

## Why Adopt These?
AsyncAPI does not constrain how Bindings are defined, beyond a valid JSON schema.  This makes Bindings flexible extension points for implementers. This flexibility is powerful, but it comes at a price in that it can be hard for implementers to understand how to use Bindings consistently, which makes it difficult for those writing tools that work with AsyncAPI to support a range of protocols.

## What Belongs on a Binding?
The Binding should be used to provide configuration data that a protocol requires. That configuration may be exposed via Message-Oriented-Middleware (MoM) using a Management API that allows code to configure the MoM as described. The fields in a Binding should represent protocol specific metadata, that is not exposed as an existing field on the Channel, Operation or Message objects.

A Binding may also be used by the providers of SDKs for MoM to provide metadata to configure producers or consumers using that SDK. That is outside the scope of advice here, but SDK owners wishing to use Bindings may follow the general advice here.

## Effective Bindings
### **Item 1** Use the Extensions Format
Prefer use of the [Bindings Extensions](https://github.com/asyncapi/extensions-catalog) format when defining a binding over raw JSON Schema.

The Binding Extensions format works with tools because it indicates where in the specification the binding should be used via a Hook. Using a Hook signals clearly to the user of an extension on which of Channel, Operation or Message the binding can be used. Implementers may choose to use author a binding so that the only fields on the binding are those appropriate to the object identified by the Hook. It also makes it possible for tools to determine correctness of an AsyncAPI file by ensuring that the binding is only used where permitted.

### **Item** 2 Prefer JSON for Bindings
Prefer the use of JSON to define a Binding. 

[The AsyncAPI specification is in JSON](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#format), so using JSON to define the Binding assures consistency of the format of the two. 

Whilst both JSON and YAML are human readable, YAML is more readable. As [YAML is a superset of JSON](https://yaml.org/spec/1.2/spec.html#id2759572) users may prefer to write AsyncAPI files in YAML, benefitting from the improved readability.  

However, as JSON is a subset of YAML, defining the Binding in YAML risks using parts of the specification not supported by JSON.  As it is possible to use a binding defined in JSON from YAML (this is true of AsyncAPI in general), authoring a binding in JSON ensures that implementers stick to the subset defined by JSON when defining the Binding.

Writing the Binding in JSON uses the common subset of JSON and YAML and should be preferred.

### **Item 3** Support Infrastructure as Code
Design your binding to support Infrastructure as Code (IaC) scenarios, so that it is possible to use the Binding with a [Generator](https://github.com/asyncapi/generator) [template](https://github.com/asyncapi/template-for-generator-templates) to generate code that will allow repeatable infrastructure for asynchronous communication. At a minimum, the binding should support the variables required by the transport's management API to create required infrastructure. 

Not all uses of a Binding have Infrastructure as Code as a goal, and not all parameters to API calls tend to be required, so it is important to avoid using required fields on any bindings where the information is likely needed in IaC or Contract-First scenarios [see Item 4](#item-4-support-contract-first--scaffolding).

Some transports do not provide a specification of a management API used to create infrastructure but instead have middleware specific APIs. To support a protocol that does not define infrastructure creation as part of its standard, consider a binding for the middleware in addition to the protocol i.e. AMQP 1-0-0.

### **Item 4** Support Contract First && Scaffolding
Design your binding to support Contract First definition of an endpoint that is later used with [Scaffolding](https://en.wikipedia.org/wiki/Scaffold_(programming)) to generate the skeleton of an application. Contract Fist scenarios requires the binding to support the necessary parameters to either produce or consume messages via the API or SDK.

Code generation from an AsyncAPI definition, particularly for consumers, is a use case that improves developer productivity and tends to drive adoption. Not all uses of a Binding have later generation of code via Scaffolding as a goal. 

There may be multiple SDKs for a particular protocol, for different languages and frameworks. In many cases the variables required by these targets remain common across the targets and can simply be inferred from the protocol. In this case it is possible to define a common set of fields for a binding that can be used by Scaffolding regardless of the SDK. In other cases, SDKs vary considerably for the middleware. In this case you may prefer to provide a binding for the middleware that facilitates code generation for that middleware. 

### **Item 5** Assume Separate Endpoints Have Separate AsyncAPI Files
Producers and consumers are usually separate apps, they may belong to separate teams, and under models such as pub-sub, there may an architectural goal to decouple producer and consumer by ensuring that each knows about the channel, but not the other. For this reason, you should assume that it will be common to define separate endpoints for the producer and consumer, but that the channel and the message schema will be shared. (Users may use $ref to prevent duplication, but that does not change these guidelines).

### **Item 6** The Channel Binding Describes the 'Virtual Pipe'
Design your binding to support the separation of the definitions of producer and consumer endpoints. The [Channel Binding](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#channelBindingsObject) should contain the data required to create a channel via [Infrastructure as Code](#item-3-support-infrastructure-as-code).

```yml
channels:
    quux:
        subscribe:
            summary: Messages about quux
            operationId: sendQuux
            $ref: "#/components/messages/quuxEvent"
    bindings:
        amqp:
            exchange:
                name: myExchange
                type: topic
                durable: true
                autoDelete: false
                vhost: /
                bindingVersion: 0.1.0
```

```yml
channels:
    quux:
        subscribe:
            summary: Messages about quux
            operationId: sendQuux
            $ref: "#/components/messages/quuxEvent"
    bindings:
        kafka:
            topic:
                partitioner: ConsistentRandom
                numOfPartitions: 6
                replicationFactor: 3
```

The Channel Binding should not contain the data required to create a subscription to the channel, or to publish to the channel. Note that the name of the channel for use by middleware is given by the name of the Channel object, which would be 'quux' in the example above.


### **Item 7** The Subscribe Operation Binding on a Producer Describes How We Send
Design your binding to support the separation of the definitions of producer and consumer endpoints. The Subscribe [Operation Binding](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#operationBindingsObject) on a producer should contain the data required to publish to a channel via [Infrastructure as Code](#item-3-support-infrastructure-as-code). Commonly, there may be reliability and security requirements for the producer. The binding should also contain any data required to support [Contract First](#item-4-support-contract-first--scaffolding) development.

```yml
channels:
    quux:
        subscribe:
            summary: Messages about quux
            operationId: sendQuux
            $ref: "#/components/messages/quuxEvent"
            bindings:
                amqp:
                    producer:
                        confirmSelect: true
    bindings:
        amqp:
            exchange:
                name: myExchange
                type: topic
                durable: true
                autoDelete: false
                vhost: /
                bindingVersion: 0.1.0
```

```yml
channels:
    quux:
        subscribe:
            summary: Messages about quux
            operationId: sendQuux
            $ref: "#/components/messages/quuxEvent"
                kafka:
                    producer:
                        transactionalId: Foo
                        replication: Acks.All
                        batchMessages: 10
                        retries: 3
                        queue_strategy: fifo
                        max_in_flight: 5
                     
    bindings:
        kafka:
            topic:
                partitioner: ConsistentRandom
                numOfPartitions: 6
                replicationFactor: 3
```

Note that the producer defines what it exposes - a subscribe operation because it can be subscribed to - so the middleware 'publication' is defined on the consumer's publish operation. This catches out some users of AsyncAPI.

### **Item 8** The Publish Operation Binding on a Consumer Describes How We Receive
Design your binding to support the separation of the definitions of producer and consumer endpoints. The Publish [Operation Binding](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#operationBindingsObject) on a consumer should contain the data required to receive from a channel via [Infrastructure as Code](#item-3-support-infrastructure-as-code). Commonly, a subscription must be made to the channel to route  messages to the consumer. There may also be security requirements for the consumer to receive messages. Where middleware uses a queue, this will typically be defined as part of the Operation Binding on the consumer. The binding should also contain any data required to support [Contract First](#item-4-support-contract-first--scaffolding) development.

```yml
channels:
    quux:
        publish:
            summary: Messages about quux
            operationId: sendQuux
            $ref: "#/components/messages/quuxEvent"
            bindings:
                amqp:
                    queue:
                        name: myQueue
                        type: topic
                        durable: true
                        autoDelete: false
                        bindingVersion: 0.1.0
    bindings:
        amqp:
            exchange:
                name: myExchange
                type: topic
                durable: true
                autoDelete: false
                vhost: /
                bindingVersion: 0.1.0
```

```yml
channels:
    quux:
        publish:
            summary: Messages about quux
            operationId: sendQuux
            $ref: "#/components/messages/quuxEvent"
            bindings:
                kafka:
                    consumer:
                        group: myGroup
                        maxPollIntervalInMs: 100
                        enableAutoOffsetStore: true
                        enableAutoCommit: true
    bindings:
        kafka:
            topic:
                partitioner: ConsistentRandom
                numOfPartitions: 6
                replicationFactor: 3
```


Note that the consumer defines what it exposes - a publish operation because it can be published to - so the middleware 'subscription' is defined on the consumer's publish operation. This catches out some users of AsyncAPI.

### **Item 9** Define Protocol Required Message Properties as Schema not Bindings
A [Message](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#messageBindingsObject) consists of headers (metadata) and payload (data); AsyncAPI also defines some common metadata values as fields of a Message, for example correlationId. The headers are defined as a [Schema](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#schemaObject) object under [Components](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#componentsObject). A payload may be of any type, but defaults to JSON Schema. [MessageTraits](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#messageTraitObject) allow the definition of metadata values related to a protocol once, rather than per message, and simplify the use of protocol related message metadata when defining message headers.

Where a protocol defines metadata that forms part of the message sent via middleware it should be defined as part of the header schema (potentially as MessageTraits), and where the protocol defines data (or transmits metadata in the payload) it should be defined as part of the payload. Note that the payload does not support MessageTraits. 

Tooling to validate that messages match the schema will expect to verify against the schema of the message payload and headers, putting message metadata into a Message Binding makes this harder as tooling must know to look there as well. 

Implementers of producers or consumers are also likely to assume that the the can refer to #/Components/Schema to understand payloads and headers, and adding metadata into bindings in addition makes it harder to comprehend the schema of a message.

A [Message Binding](https://github.com/asyncapi/spec/blob/master/spec/asyncapi.md#messageBindingsObject) should be used to define metadata about the protocol's message format that is used for [Infrastructure as Code](#item-3-support-infrastructure-as-code) or [Scaffolding](#item-4-support-contract-first--scaffolding). Don't use the Message Bindings to describe what is sent; use the Message Binding to describe how it is sent, if required.

```yml
channels:
    quux:
        subscribe:
            summary: Messages about quux
            operationId: sendQuux
            $ref: "#/components/messages/quuxEvent"

...

components:
    schemas:
       quux:
	 type: object
	 properties:
	     quuxDetails: 
	     	type: string	 
       amqpBasicProperties:
               type: object
	       properties:
                   content_encoding:
		      type: string
		   delivery-mode:
		      type: integer
		      minimum: 0
		      maximum: 1
		   priority:
		      type: integer
		      minimum: 0
		      maximum: 9
		   correlation-id:
		      type: string
		   reply-to:
		      type: string
		   expiration:
		      type: string
		   message-id:
		      type: string
		   timestamp:
		      type: string
		   type:
		       type: string
		   user-id:
		       type: string
		   app-id:
		       type: string
       
    messages:
        quuxEvent:
      	    summary: Raised when a quux occurs.
            description: When a quux happens, describes the quux		  
      contentType: application/json
      payload:
        $ref: "#/components/schemas/quux"
      traits:
        $ref: "#/components/schemas/amqpBasicProperties" 


```

### **Item 10** Be Consistent with Other Protocols
Design your binding with the possibility that a producer might expose a message over different bindings - so as to support a wider range of consumers. By following the guidelines in this document your binding should be compatible with other bindings, and simplify the development of tooling that supports those because they operate at the same level.

```yml
channels:
    quux:
        subscribe:
            summary: Messages about quux
            operationId: sendQuux
            $ref: "#/components/messages/quuxEvent"
            bindings:
                amqp:
                    producer:
                        confirmSelect: true
                kafka:
                    producer:
                        transactionalId: Foo
                        replication: Acks.All
                        batchMessages: 10
                        retries: 3
                        queue_strategy: fifo
                        max_in_flight: 5
                     
    bindings:
        amqp:
            exchange:
                name: myExchange
                type: topic
                durable: true
                autoDelete: false
                vhost: /
                bindingVersion: 0.1.0
        kafka:
            topic:
                partitioner: ConsistentRandom
                numOfPartitions: 6
                replicationFactor: 3
```


