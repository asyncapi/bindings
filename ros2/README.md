# ROS 2 Bindings

This document defines how to describe ROS 2-specific information in AsyncAPI.

It applies to all [distributions of ROS 2](https://docs.ros.org/en/rolling/Releases.html).

<a name="version"></a>

## Version

Current version is `0.1.0`.

<a name="server"></a>

## Server Binding Object

This object contains information about the server representation in ROS 2. 
ROS 2 can use either DDS or Zenoh as its middleware. 
DDS is decentralized with no central server, so the field `host` can be set to `localhost`.
When using Zenoh, the `host` field specifies the Zenoh Router IP address.

###### Fixed Fields

Field Name | Type | Description
---|:---:|---|
`rmwImplementation` | string  |  Specifies the ROS 2 middleware implementation to be used. Valid values include the different [ROS 2 middleware vendors](https://docs.ros.org/en/rolling/Concepts/Intermediate/About-Different-Middleware-Vendors.html). This determines the underlying middleware implementation that handles communication.
`domainId` | integer  |  All ROS 2 nodes use domain ID 0 by default. To prevent interference between different groups of computers running ROS 2 on the same network, a group can be set with a unique domain ID. Must be a non-negative integer less than 232. 

### Examples

```yaml
servers:
  ros2:
    host: localhost
    protocol: ros2
    protocolVersion: humble
    bindings:
      ros2:
        rmwImplementation: rmw_fastrtps_cpp
        domainId: 0
```


<a name="channel"></a>

## Channel Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.

<a name="operation"></a>

## Operation Binding Object

This object contains information about the ROS 2 node.

###### Fixed Fields

Field Name | Type | Description
---|:---:|---|
`role` | string | Specifies the ROS 2 type of the node for this operation. If the action is `send`, valid values for the role are: `publisher`, `action_client`, `service_client`. If the action is `receive`, valid values for the role are: `subscriber`, `action_server`, `service_server`. This defines how the node will interact with the associated topic, service or action.
`node` | string | The name of the ROS 2 node that implements this operation. 
`qosPolicies` | [Quality of Service Policy Object](#QoSPolicyObject) | Quality of Service (QoS) for the topic.

<a name="QoSPolicyObject"></a>

### Quality of Service Object
This object contains ROS 2 specific information about the Quality of Service policies.
More information here: https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies

Field Name | Type | Description
---|:---:|---|
`reliability` | string | One of `best_effort` or `reliable`.  More information here: [ROS 2 QoS](https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies)
`history` | string | One of `keep_last`, `keep_all` or `unknown`. More information here: [ROS 2 QoS](https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies)
`durability` | string | One of `transient_local` or `volatile`. More information here: [ROS 2 QoS](https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies)
`lifespan` | integer |  The maximum amount of time between the publishing and the reception of a message without the message being considered stale or expired. `-1` means infinite. 
`deadline` | integer | The expected maximum amount of time between subsequent messages being published to a topic. `-1` means infinite.
`liveliness` | string | One of `automatic` or `manual`. More information here: [ROS 2 QoS](https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies)
`leaseDuration` | integer | The maximum period of time a publisher has to indicate that it is alive before the system considers it to have lost liveliness. `-1` means infinite.

### Examples

ROS 2 subscriber example:

```yaml
  receiveCmdVel:
    action: receive
    channel:
      $ref: "#/channels/CmdVel"
    bindings:
      ros2:
        role: subscriber
        node: /turtlesim
          qosPolicies:
            history: unknown
            reliability: reliable
            durability: volatile
            lifespan: -1
            deadline: -1
            liveliness: automatic
            leaseDuration: -1
```

ROS 2 publisher example:
```yaml
 Pose:
    action: receive
    channel:
      $ref: "#/channels/Pose"
    bindings:
      ros2:
        role: publisher
        node: /turtlesim
```

ROS 2 service server example:
```yaml
SetPen:
    action: receive
    channel:
      $ref: "#/channels/SetPenRequest"
    reply:
      channel:
        $ref: "#/channels/SetPenReply"
    bindings:
      ros2:
        role: service_server
        node: /turtlesim
```

ROS 2 service client example:
```yaml
SetPen:
    action: send
    channel:
      $ref: "#/channels/SetPenRequest"
    reply:
      channel:
        $ref: "#/channels/SetPenReply"
    bindings:
      ros2:
        role: service_client
        node: /node_client
```

ROS 2 action server example:
```yaml
receiveRotateAbsolute:
  action: receive
  channel:
    $ref: "#/channels/RotateAbsoluteRequest"
  reply:
    channel:
      $ref: "#/channels/RotateAbsoluteReply"
  bindings:
    ros2:
      role: action_server
      node: /turtlesim
```

ROS 2 action client example:
```yaml
RotateAbsolute:
    action: send
    channel:
      $ref: "#/channels/RotateAbsoluteRequest"
    reply:
      channel:
        $ref: "#/channels/RotateAbsoluteReply"
    bindings:
      ros2:
        role: action_client
        node: /teleop_turtle
```

<a name="message"></a>

## Message Binding Object

This object MUST NOT contain any properties. Its name is reserved for future use.