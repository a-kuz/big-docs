# Requests with responses

The following tables represent the structure for various payloads, client-server requests and responses.

## payload types

### new

**request payload schema:**

| Field           | Type    | Example                                  | Possible Values             |
| --------------- | ------- | ---------------------------------------- | --------------------------- |
| chatId          | string  | "User2"                                  | Chat IDs                    |
| message         | string  | "Hello, World!"                          | Any string                  |
| clientMessageId | string  | "66d93f9b-a8ff-4f18-a092-c19bdeb31fa4"   | Any string                  |
| attachments?    | array   | See ["Attachments"](types/attachment.md) | Array of Attachment objects |
| replyTo?        | integer | 1                                        |

**response payload schema:**

| Field           | Type    | Example                                | Possible Values    |
| --------------- | ------- | -------------------------------------- | ------------------ |
| messageId       | integer | 123                                    | message seq number |
| timestamp       | integer | 1700500000000                          |                    |
| clientMessageId | string  | "66d93f9b-a8ff-4f18-a092-c19bdeb31fa4" | Any string         |

### dlvrd

set mark "delivered" to message.

**request payload schema:**

| Field      | Type    | Example | Possible Values |
| ---------- | ------- | ------- | --------------- |
| chatId     | string  | "User2" | Chat IDs        |
| messageId? | integer | 123     | Message IDs     |

message id is optional, if not specified, all messages will be marked as delivered

**response payload schema:**

| Field     | Type    | Example       | Possible Values |
| --------- | ------- | ------------- | --------------- |
| messageId | integer | 123           | Message IDs     |
| timestamp | integer | 1700500000000 |                 |

### read

set mark "read" to message

| Field      | Type    | Example | Possible Values |
| ---------- | ------- | ------- | --------------- |
| chatId     | string  | "User2" | Chat IDs        |
| messageId? | integer | 123     | Message IDs     |

message id is optional, if not specified, all messages will be marked as read

**response payload schema:**

| Field     | Type    | Example       | Possible Values |
| --------- | ------- | ------------- | --------------- |
| messageId | integer | 123           | Message IDs     |
| timestamp | integer | 1700500000000 |                 |
| missed    | integer | 2             |                 |

/// details | read / dlvrd optimizations

“read” and “delivered” marks are placed on all messages whose id <= transmitted, if they are not already marked as “read” and “delivered”. For example, you have 5 new messages. you can pass id = 5 and all 5 will be marked as read. If you pass id = 4, the fifth will remain unread
///

---

### messages

**request payload schema:**

(1)
{ .annotate }

  1. **endIs is optional**

    ```json
      {
        "chatId": "User2",
        "count": 50
      }

    ```
  
| Field  | Type    | Example | Possible Values               |
| ------ | ------- | ------- | ----------------------------- |
| chatId | string  | "User2" | Chat IDs                      |
| endId? | integer | 199     | Message IDs                   |
| count? | integer | 50      | 1 - ∞, default - all messages |

**response payload schema:**

[Message](types/message.md)[]

## chat

**request payload schema:**

| Field  | Type   | Example | Possible Values |
| ------ | ------ | ------- | --------------- |
| chatId | string | "User2" | Chat IDs        |

**response payload schema:**

[Chat](types/chat.md)

## chats

**request payload schema:**

| Field | Type | Example |
| ----- | ---- | ------- |

**response payload schema:**

[ChatList](types/chat-list.md)
