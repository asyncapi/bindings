# ROS 2 Bindings

This document defines how to describe ROS 2-specific information in AsyncAPI.

It applies to all versions of ROS 2 (foxy, galactic, humble, iron, jazzy).

<a name="version"></a>

## Version

Current version is `0.1.0`.

<a name="server"></a>

## Server Binding Object

This object contains information about the server representation in ROS 2. 
ROS 2 can use either DDS or Zenoh as its middleware. 
DDS is decentralized with no central server, so the field `host` can set to `localhost`.
When using Zenoh, the `host` field specifies the Zenoh Router IP address.

###### Fixed Fields

Field Name | Type | ROS 2 Versions | Description
---|:---:|:---:|---|
`rmwImplementation` | string  | all | Specifies the ROS 2 middleware implementation to be used. Valid values include `rmw_fastrtps_cpp` (Fast DDS), `rmw_cyclonedds_cpp` (Cyclone DDS), `rmw_connext_cpp` (RTI Connext), and `rmw_zenoh_cpp` (Zenoh). This determines the underlying middleware implementation that handles communication.
`domainId` | integer  | all | All ROS 2 nodes use domain ID 0 by default. To prevent interference between different groups of computers running ROS 2 on the same network, a group can be set with a unique domain ID. Must be a non-negative integer less than 232. 

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

A channel represents a ROS 2 topic, service or action interface.

This object DOES NOT contain any ROS 2 specific properties.

Example - ROS 2 topic:

```yaml
  address: /turtle1/cmd_vel
    messages:
      TwistMsg:
        $ref: '#/components/messages/TwistMsg'
```
Example  - ROS 2 action:

```yaml
  RotateAbsoluteRequest:
    address: /turtle1/rotate_absolute
    messages:
      RotateAbsoluteActionRequest:
        $ref: '#/components/messages/RotateAbsoluteActionRequest'
  
  RotateAbsoluteReply:
    address: /turtle1/rotate_absolute
    messages:
      RotateAbsoluteActionResult:
        $ref: '#/components/messages/RotateAbsoluteActionResult'
      RotateAbsoluteActionFeedback:
        $ref: '#/components/messages/RotateAbsoluteActionFeedback'
```

<a name="operation"></a>

## Operation Binding Object

AsyncAPI operations with their `send` and `receive` actions map directly to ROS 2 subscribers, publishers, actions or services.
- send -> `publisher`, `action_client`, `service_client`
- receive -> `subscriber`, `action_server`, `service_server`

###### Fixed Fields

Field Name | Type | ROS 2 Versions | Description
---|:---:|:---:|---|
`type` | string | all | Specifies the ROS 2 type of the node for this operation. Valid values are: `publisher`, `subscriber`, `service_client`, `service_server`, `action_client`, `action_server`. This defines how the node will interact with the associated topic or action.
`node` | string | all | The name of the ROS 2 node that implements this operation. 
`qosPolicies` | object | all | Quality of Service (QoS) for the topic.

### Quality of Service Object
This object contains ROS 2 specific information about the Quality of Service policies.
More information here: https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies

Field Name | Type | ROS 2 Versions | Description
---|:---:|:---:|---|
`reliability` | string | all | One of `best_effort` or `reliable`.  More information here: [ROS 2 QoS](https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies)
`history` | string | all | One of `keep_last`, `keep_all` or `unknown`. More information here: [ROS 2 QoS](https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies)
`durability` | string | all | One of `transient_local` or `volatile`. More information here: [ROS 2 QoS](https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies)
`lifespan` | integer | all | The maximum amount of time between the publishing and the reception of a message without the message being considered stale or expired. `-1` means infinite. 
`deadline` | integer | all | The expected maximum amount of time between subsequent messages being published to a topic. `-1` means infinite.
`liveliness` | string | all | One of `automatic`or `manual`. More information here: [ROS 2 QoS](https://docs.ros.org/en/jazzy/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-policies)
`leaseDuration` | integer | all | the maximum period of time a publisher has to indicate that it is alive before the system considers it to have lost liveliness. `-1` means infinite.
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
ROS 2 action Server example:

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


<a name="message"></a>

## Message Binding Object

ROS 2 message types (defined in .msg/.srv/.aciton files) are mapped to AsyncAPI message payloads.

This object DOES NOT contain any ROS 2 specific properties.

```yaml
Vector3Msg:
  type: object
  properties: 
    x:
      type: number
      format: double
    y:
      type: number
      format: double
    z:
      type: number
      format: double
```

ROS 2  Type | AsyncAPI Type | AsyncAPI Format | 
---|:---:|---|
bool | boolean | boolean
byte | string | octet
char | integer | uint8
float32 | number | float
float64 | number | double
int8 | integer | int8
uint8 | integer | uint8
int16 | integer | int16
uint16 | integer | uint16
int32 | integer | int32
uint32 | integer | uint32
int64 | integer | int64
uint64 | integer | uint64
string | string | string
array | array | --