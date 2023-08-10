# Retained Message Specification

## Overview

## Requirements

1. Each group **MUST** have a flag to indicate whether it has a retained message or not.
2. A retained message **MUST** have a flag to indicate whether it is a retained message.

   - Messages without a retained flag **MUST NOT** remove or replace any existing retained message.

   - Messages with a retained flag **MUST** be sent to clients that are in the group.

3. The service **MUST** send the retained message to the client immediately upon joining the group.

   - Retained messages **MUST** be sent in the same order as the groups if multiple groups were joined.

4. Each group could have **AT MOST 1** retain message.

   - The retained message **MUST** be replaced if a retained message has been sent to a group that already has a retained message.

5. A retained message **MUST** have **the same QoS** as a normal message.

6. A retained message with a payload containing 0 bytes **MUST NOT** be stored as a retained message.

   - The retained message **MUST** be removed if a retained message with a payload containing 0 bytes has been sent to the group.

7. Messages can be retained **AT MOST 30 DAYS**.

   - A retained message **MUST** be removed if a retained message was set 30 days prior.

8. A client **MUST** have permissions to send messages to a group to send a retained message.

   - The retained message **MUST NOT** be set/replaced/removed if the client doesn't have the permissions to send messages to the group.

## Usage

### Send a retained message

Sending a retained message is quite simple and straight-forward.

- Send a message with the `retained` flag set to **true** to a group. The message will be published to clients that has subscribed the group, and new connections will also receive this message immediately upon joining the group.

#### `json.webpubsub.azure.v1`

```json
{
    "type": "sendToGroup",
    "group": "<group_name>",
    "dataType" : "text",
    "data": "text data",
    "retained": true,
}
```

### Delete a retained message

Deleting a retained message is also simple.

- Send a retained message with a **0-byte payload** to the group that you want to delete the previous retained message.

#### `json.webpubsub.azure.v1`

```json
{
    "type": "sendToGroup",
    "group": "<group_name>",
    "dataType" : "text",
    "data": "",
    "retained": true,
}
```