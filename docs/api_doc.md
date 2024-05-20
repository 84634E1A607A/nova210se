# Nova 210 SE: API Documentation

This document describes the API endpoints and their usage in the Nova 210 SE project.

## Menu

```python
urlpatterns = [
    # User control
    path('user/login', user.login, name='user_login'),
    path('user/register', user.register, name='user_register'),
    path('user/logout', user.logout, name='user_logout'),
    path('user', user.query, name='user'),
    path('user/<int:_id>)', user.get_user_info_by_id, name='user_by_id'),

    # Friend group control
    path('friend/group/add', friend_group.add, name="friend_group_add"),
    path('friend/group/<int:group_id>', friend_group.query, name='friend_group_query'),
    path('friend/group/list', friend_group.list_groups, name='friend_group_list'),

    # Friend control
    path('friend/find', friend.find, name='friend_find'),
    path('friend/invite', friend.send_invitation, name='friend_invite'),
    path('friend/invitation', friend.list_invitation, name='friend_list_invitation'),
    path('friend/invitation/<int:invitation_id>', friend.respond_to_invitation, name='friend_respond_to_invitation'),
    path('friend', friend.list_friend, name='friend_list_friend'),
    path('friend/<int:friend_user_id>', friend.query, name='friend_query'),

    # Chat control
    path('chat/new', chat.new_chat, name='chat_new'),
    path('chat/<int:chat_id>/invite', chat.invite_to_chat, name='chat_invite'),
    path('chat/<int:chat_id>/invitation', chat.list_invitation, name='chat_list_invitation'),
    path('chat/<int:chat_id>/invitation/<int:user_id>', chat.respond_to_invitation, name='chat_respond_to_invitation'),
    path('chat/<int:chat_id>', chat.query_chat, name='chat_get_delete'),
    path('chat', chat.list_chats, name='chat_list'),
    path('chat/<int:chat_id>/<int:member_id>/admin', chat.set_admin, name='chat_set_admin'),
    path('chat/<int:chat_id>/set_owner', chat.set_owner, name='chat_set_owner'),
    path('chat/<int:chat_id>/<int:member_id>', chat.remove_member, name='chat_remove_member'),
    path('chat/<int:chat_id>/messages', chat.get_messages, name='chat_list_messages'),
    path('chat/<int:chat_id>/filter', chat.filter_messages, name='chat_filter_messages'),

    # Catch all and return 404
    re_path('.*?', api_utils.not_found, name='not_found'),
]
```

## Structs returned by APIs

### User

#### User.Basic

```json
{
    "id": 1,
    "user_name": "UserN@meWithoutBlankSpace",
    "avatar_url": "data:image/png;base64,base64encodedstring | https://example.com/avatar.png"
}
```

#### User.Detailed

```json
{
    "id": 1,
    "user_name": "UserN@meWithoutBlankSpace",
    "avatar_url": "data:image/png;base64,base64encodedstring | https://example.com/avatar.png",
    "email": "",
    "phone": "12345678901"
}
```

### FriendGroup

```json
{
    "group_id": 1,
    "group_name": "Simple Friend Gr0up~"
}
```

### Friend

```json
{
    "friend": `User.Detailed`,
    "nickname": "Bla~ Bla~ Bla",
    "group": `FriendGroup`
}
```

### FriendInvitation

```json
{
    "id": 1,
    "sender": `User.Basic`,
    "receiver": `User.Basic`,
    "comment": "#!*(&$87)r6h#(*asL0#*%&)sdfaNN1&^",
    "source": `Chat.id | "search"`
}
```

### Chat

```json
{
    "chat_id": 1,
    "chat_name": "Name Of Chat",
    "chat_owner": `User.Basic`,
    "chat_admins": [`User.Basic`, ...],
    "chat_members": [`User.Basic`, ...],
    "last_message": `ChatMessage.Basic | ""`
}
```

### UserChatRelation

```json
{
    "chat": `Chat`,
    "nickname": "Nick for Chat",
    "unread_count": 0
}
```

### ChatInvitation

```json
{
    "invitation_id": 1,
    "chat_id": 2,
    "user": `User.Basic`,
    "invited_by": `User.Basic`,
    "created_at": 192837192.123
}
```

### ChatMessage

#### ChatMessage.Basic

```json
{
    "message_id": 1,
    "chat_id": 3,
    "message": "Hello~",
    "send_time": 192837192.456,
    "sender": `User.Basic`,
    "reply_to_id": `null | 3`,
    "deleted": false
}
```

#### ChatMessage.Detailed

```json
{
    "message_id": 1,
    "chat_id": 3,
    "message": "Hello~",
    "send_time": 192837192.456,
    "sender": `User.Basic`,
    "reply_to_id": `null | 3`,
    "deleted": false,
    "read_users": [`User.Basic`, ...],
    "reply_to": `ChatMessage.Basic | null`,
    "replied_by": [`ChatMessage.Basic`, ...]
}
```

## User control APIs

The User Control APIs are defined in the `main.views.user` module.

### [`login`](main/views/user.py#login)

This function handles the `POST /user/login` endpoint. It logs in to a user account. This API accepts a POST request with JSON content. An example of which is:

```json
{
  "user_name": "user",
  "password": "password"
}
```

The API returns the user information if the login is successful and will set session cookies for the user. A successful response looks like:

```json
{
  "ok": true,
  "data": `User.Detailed`
}
```

If the username doesn't exist or the password is incorrect, the API returns an error message with 403 status code, like:

```json
{
  "ok": false,
  "error": "Invalid username or password"
}
```

### [`get_user_info`](main/views/user.py#get_user_info)

This function handles the `GET /user` endpoint. It returns the user information for the current user. The returned structure is the same as the login API.

### [`query`](main/views/user.py#query)

This function handles the `GET, PATCH, DELETE /user` endpoints. This API requires a valid session cookie to be sent with the request. It accepts GET, PATCH, and DELETE requests.

- GET request returns the user information.
- PATCH request updates the user information.
- DELETE request deletes the user.

#### [`GET query`](main/views/user.py#get_user_info)

The GET request returns the user information for the current user. The returned structure is the same as the login API.

#### [`PATCH query`](main/views/user.py#edit_user_info)

The PATCH request updates the user information. The API accepts a JSON object with the fields to be updated. An example of the JSON object is:

```json
{
  "old_password": "old password",
  "new_password": "new password", // Optional
  "user_name": "new user name", // Optional
  "email": "new_email@example.com", // Optional
  "phone": "15912345678", // Optional
  "avatar_url": "https://..." // Optional
}
```

old_password is required if and only if new_password, email _or_ phone is present. If old_password is incorrect, the API returns 403 status code.
If new_password is present, the API updates the password and the session cookies (logs the user out and back in).

If the new password doesn't conform to the password requirements, the API returns 400 status code with an error message.

If email is present, the API updates the email. If the email is longer than 100 characters, or it doesn't match the email format, the API returns 400 error code.

If phone is present, the API updates the phone number. If the phone number is invalid, the API returns 400.

If email or phone is set to "", this API accepts it.

Username can be changed, but it must conform to the username requirements. (See register API for details)

If the avatar_url is present, the API updates the avatar URL. If the URL is longer than 490 characters, or it doesn't start with `http(s)://`, the API returns 400 error code.

If the avatar_url is set to "", this API accepts it and generates a random avatar for the user.

All changes are applied if and only if all checks are passed. That is to say, if any error code is returned, none of the requested changes are applied.

This API returns the user information (like login page) after the update.

#### [`DELETE query`](main/views/user.py#delete_user)

Delete the user logged in and log him out.

This API returns 200 status code with an empty data field if the deletion is successful.

### [`logout`](main/views/user.py#logout)

This function handles the `POST /user/logout` endpoint. This API requires a valid session cookie to be sent with the request. It logs the user out and clears the session. If no valid session is found, the API returns 403 status code with an error message. The API returns 200 status code with an empty data field if the logout is successful.

## Friend Group control APIs

Friend Group Control APIs are defined in the `main.views.friend_group` module.

### [`add`](main/views/friend_group.py#add)

This function handles the `POST /friend/group/add` endpoint. This API requires a valid session cookie to be sent with the request. It creates a new friend group for the user. The API accepts a JSON object with the group name. An example of the JSON object is:

```json
{
  "group_name": "group name"
}
```

The API returns the group information if the group is created successfully. The returned structure is:

```json
{
  "ok": true,
  "data": `FriendGroup`
}
```

If the group name is empty or longer than 50 characters, the API returns 400 status code with an error message.

### [`query`](main/views/friend_group.py#query)

This function handles the `GET, PATCH, DELETE /friend/group/<int:group_id>` endpoints. This API requires a valid session cookie to be sent with the request. It accepts GET, PATCH, and DELETE requests.

#### [`GET query`](main/views/friend_group.py#get_group_by_id)

The GET request returns the group information for the given group_id. The returned structure is:

```json
{
  "ok": true,
  "data": `FriendGroup`
}
```

If the group_id doesn't exist, the API returns 404 status code with an error message.

If the user doesn't have access to the group, the API returns 403 status code with an error message.

#### [`PATCH query`](main/views/friend_group.py#edit)

The PATCH request updates the group information. It updates the group name and returns the group information if the group is found and belongs to the user.

A successful response looks the same as that of the add function.

If the group is not found, the API returns 400 status code.

If the group does not belong to the user, the API returns 403 status code.

The name of user's default group cannot be changed. If an attempt is made to change the name of the default group, this API will return 400 status code.

#### [`DELETE query`](main/views/friend_group.py#delete)

The DELETE request deletes the group if the group is found and belongs to the user.

This API returns 200 status code with an empty data field if the deletion is successful, like:

```json
{
  "ok": true,
  "data": null
}
```

If the group is not found, the API returns 400 status code.

If the group does not belong to the user, the API returns 403 status code.

If the group is non-empty, all users in the group will be moved to the default group.

The default group cannot be deleted. If an attempt is made to delete the default group, this API will return 400 status code.

### [`list_groups`](main/views/friend_group.py#list_groups)

This function handles the `GET /friend/group/list` endpoint. This API requires a valid session cookie to be sent with the request. It returns a list of groups for the user. The returned structure is:

```json
{
  "ok": true,
  "data": [
    {
      "group_id": 1,
      "group_name": "" // Default group with empty name
    },
    `FriendGroup`, ...
  ]
}
```

## Friend control APIs

Friend Control APIs are defined in the `main.views.friend` module.

### [`find`](main/views/friend.py#find)

This function handles the `POST /friend/find` endpoint.

Find user by its ID / name. Returns a list of filtered users without the current user like:

```json
{
    "ok": true,
    "data": [`User.Basic`, ...]
}
```

This api requires a valid session, or it will return a 403 response.

Possible filters are `id` and `name_contains`. If id is provided, the API performs a precise lookup. If a user with the given id is found, the API will return a list with only one item; else the API will return an empty list. If name_contains is provided, the API performs a case-sensitive search. Any user with a name _containing_ the given string will be returned.

If both id and name_contains are provided, the API will _only_ use id to perform the lookup.

The current user and system users will not be returned in the list.

If none of the filters are provided, the API returns 400 status code.

### [`send_invitation`](main/views/friend.py#send_invitation)

This function handles the `POST /friend/invite` endpoint. This API requires a valid session cookie to be sent with the request.

If a pending invitation is found from the receiver to the sender, the API will accept the invitation and return the created friendship; source is not validated in this case.

This API return 200 status code with empty data field if the invitation is sent successfully.

id, comment and source are required fields, and 400 response is returned if any of them is missing / type is incorrect.

If the requested user does not exist, the API returns 400 status code with an error message.

If the requested user is already the online user's friend, the API returns 409 status code with an error message.

"source" can be either "search" (of type string) or a group id (of type int, not implemented yet). Any other value will result in a 400 response.

Comment should be less than 500 characters, or the API will return 400 status code with an error message.

### [`list_invitation`](main/views/friend.py#list_invitation)

This function handles the `GET /friend/invitation` endpoint. This API requires a valid session cookie to be sent with the request. It returns a list of friend invitations related to the current user. The returned structure is:

```json
{
  "ok": true,
  "data": [`FriendInvitation`, ...]
}
```

### [`respond_to_invitation`](main/views/friend.py#respond_to_invitation)

This function handles the `POST, DELETE /friend/invitation/<int:invitation_id>` endpoints. This API requires a valid session cookie to be sent with the request. It accepts POST and DELETE requests.

- POST request accepts the invitation and creates a friendship. It returns the friend information if the invitation is accepted.
- DELETE request rejects the invitation.

If the invitation is not found, the API returns 400 status code with an error message.

If the invitation was found but the receiver is not the current user, the API returns 403 status code with an error message.

### [`query`](main/views/friend.py#query)

This function handles the `GET, PATCH, DELETE /friend/<int:friend_user_id>` endpoints. This API requires a valid session cookie to be sent with the request. It accepts GET, PATCH, and DELETE requests.

#### [`GET query`](main/views/friend.py#get_friend_info)

The GET request returns the friend information for the given friend_user_id. The returned structure is:

```json
{
  "ok": true,
  "data": `Friend`
}
```

If the friend is not found, the API returns 404 status code with an error message.

#### [`PATCH query`](main/views/friend.py#update_friend)

The PATCH request updates the friend information. It updates the nickname and group of the friend. The API returns the updated friend information if the update is successful.

If the friend is not found, the API returns 400 status code; or it checks if "nickname" or "group_id" is provided.

If "nickname" is provided, the API tries to updates the nickname. If the "nickname" is not a string, function returns 400. The nickname should be less than 100 characters, or the API returns 400.

If "group_id" is provided, the API tries to updates the group. However, if the group does not exist or does not belong to the user, 400 and 403 is returned respectively.

Friend information will be updated if and only if no errors occur.

If the update is successful, the API returns the updated friend information in the same format as the get function.

#### [`DELETE query`](main/views/friend.py#delete_friend)

The DELETE request deletes the friend. This API returns 200 status code with an empty data field if the deletion is successful.

If the friend is not found, the API returns 400 status code with an error message.

### [`list_friend`](main/views/friend.py#list_friend)

This function handles the `GET /friend` endpoint. This API requires a valid session cookie to be sent with the request. It returns a list of friends for the current user. The returned structure is:

```json
{
  "ok": true,
  "data": [`Friend`, ...]
}
```

## Chat control APIs

### [`new_chat`](main/views/chat.py#new_chat)

This function handles the `POST /chat/new` endpoint. This API requires a valid session cookie to be sent with the request. It creates a new chat with the given users. This API accepts a POST request with JSON content. An example of which is:

```json
{
  "chat_name": "chat name",
  "chat_members": [1, 2, 3]
}
```

"members" should be a list of user ids. The chat will be created with the current user as the owner, and the users with the given ids as members. The current user will be added to the chat as a member regardless of whether the user id is in the "members" list.

Each member MUST be an existing user and a friend to the current user, or the API will return 400.

The API returns the chat information if the chat is created successfully. A successful response looks like:

```json
{
    "ok": true,
    "data": `Chat`
}
```

### [`invite_to_chat`](main/views/chat.py#invite_to_chat)

This function handles the `POST /chat/<chat_id>/invite` endpoint. This API requires a valid session cookie to be sent with the request. It invites a user to a _group chat_. This API accepts a POST request with JSON content. An example of which is:

```json
{
  "user_id": 1
}
```

You can invite users only in a group chat. If the chat is a private chat, the API will return 400.

A notification will be sent to group owner and admins for them to approve / decline the invitation, but the user invited WILL NOT RECEIVE A NOTIFICATION.

The user MUST be an existing user and a friend to the current user, or the API will return 400.

### [`list_invitation`](main/views/chat.py#list_invitation)

This function handles the `GET /chat/<chat_id>/invitation` endpoint. This API requires a valid session cookie to be sent with the request. It lists all pending invitations in a chat. It retues a list of invitations in the chat, each in the format of ChatInvitation.to_struct

Current user must be the chat owner or an admin to view the invitations.

If the chat does not exist, the API will return 400.

If the user is neither the owner nor an admin of the chat, the API will return 403.

### [`respond_to_invitation`](main/views/chat.py#respond_to_invitation)

This function handles the `POST, DELETE /chat/<chat_id>/invitation/<user_id>` endpoints. This API requires a valid session cookie to be sent with the request. It approves / declines a chat invitation. It accepts POST and DELETE requests.

You can only approve / decline an invitation in a group chat and as the group owner or an admin.

A successful response will return 200 status code with an empty data field.

If the chat does not exist, the API will return 400.

If the user is neither the owner nor an admin of the chat, the API will return 403.

If the invitation does not exist, the API will return 400.

- POST: Expects an empty body, the user will be added to the chat, and _then_ magic user #SYSTEM will send a message there. If any other member had sent an invitation to the same user, the invitation will be deleted.
- DELETE: The invitation will be deleted.

### [`list_chats`](main/views/chat.py#list_chats)

This function handles the `GET /chat` endpoint. This API requires a valid session cookie to be sent with the request. It returns a list of chats that the current user is in. The returned structure is:

```json
{
  "ok": true,
  "data": [`UserChatRelation`, ...]
}
```

### [`query_chat`](main/views/chat.py#query_chat)

This function handles the `GET, DELETE /chat/<chat_id>` endpoints. This API requires a valid session cookie to be sent with the request. It accepts GET and DELETE requests.

If the chat does not exist, the API will return 400.

- GET request returns the chat information in the format of UserChatRelation.to_struct.
- DELETE request removes the current user from the chat.
  - If the current user is the owner of the chat, the whole chat along with all chat messages will be permanently deleted.
  - If the current user is not the owner of the chat, the user will be removed from the chat, and all chat messages sent by the user will preserve. A system message will be sent to the chat to notify the members that the user has left the chat.
  - The API returns 200 status code with an empty data field if the chat is deleted successfully.

### [`get_messages`](main/views/chat.py#get_messages)

This function handles the `GET /chat/<chat_id>/messages` endpoint. This API requires a valid session cookie to be sent with the request. It returns all messages in a chat. The returned structure is a list of messages in the chat, each in the format of ChatMessage.to_detailed_struct.

If the chat does not exist, the API will return 404; if the user is not in the chat, the API will return 403.

### [`filter_messages`](main/views/chat.py#filter_messages)

This function handles the `POST /chat/<chat_id>/filter` endpoint. This API requires a valid session cookie to be sent with the request. It filters messages in a chat given filter conditions. This API accepts a POST request with JSON content. An example of which is:

```json
{
  "begin_time": 192937123.342, // UNIX timestamp, optional
  "end_time": 192937123.342, // UNIX timestamp, optional
  "sender": [1, 2, 3] // List of user ids, optional
}
```

begin_time, end_time and sender are all optional, but if none of them are provided, the API will return 400.

begin_time and end_time are not type-strict, but they must be convertible to a float. If the conversion fails, the API will return 400.

#SYSTEM and #DELETED can be used as sender ids.

The API will return a list of messages that satisfy the filter conditions. Each message will be in the format of ChatMessage.to_detailed_struct, ordered by send time descendent.

### [`set_admin`](main/views/chat.py#set_admin)

This function handles the `POST /chat/<chat_id>/<member_id>/admin` endpoint. This API requires a valid session cookie to be sent with the request. It sets a user as an / not an admin of a chat. This API accepts a POST request with JSON content. An example of which is:

```json
true
```

You can only set a user as an admin in a group chat as the chat owner.

The user with the given member_id will be set as an / as not an admin of the chat with the given chat_id.

The chat owner cannot be set as an admin / not an admin.

If the user is already an admin / not an admin, the API will return 400.

The API returns 200 status code with an empty data field if operation is successful.

### [`set_owner`](main/views/chat.py#set_owner)

This function handles the `POST /chat/<chat_id>/set_owner` endpoint. This API requires a valid session cookie to be sent with the request. It sets a new owner of a chat. This API accepts a POST request with JSON content. An example of which is:

```json
{
  "chat_owner": 1
}
```

You can only transfer the owner of a group chat as the chat owner.

The user with the given user_id will be set as the new owner of the chat with the given chat_id. If the user was an admin, the user will be removed from the admin list. After the operation, the current user will be added to the admin list.

If the user is already the owner (you cannot transfer owner to yourself) / the user is not in the group, the API will return 400.

### [`remove_member`](main/views/chat.py#remove_member)

This function handles the `DELETE /chat/<chat_id>/<member_id>` endpoint. This API requires a valid session cookie to be sent with the request. It removes a member from a chat. This API accepts a DELETE request.

You can only remove a member from a group chat.

You MUST be at least an admin to remove a member; You MUST be the chat owner to remove an admin.

If the operation completes successfully, the API will return 200 with an empty data field.

A system message is posted to the chat to notify the members that the user has been removed.

## WebSocket Endpoint

The WebSocket endpoint is `/ws/`.

Authorization (session cookie) is required to connect to websocket interface,
or the server returns a json response with code 403 and disconnects.

### Client to Server packets:

All client-to-server packet should conform to the following format:

```json
{
    "action": "ACTION_NAME",
    "request_id": 0, // Optional, helps to identify response
    "data": any
}
```

Where "action" helps server to identify the intent of the packet; and data type is interpreted based on action.
If the client anticipates a response, set request_id to a certain random number and server will return
the corresponding response with request_id field set.

If the client sends an invalid packet, the server will return an error packet (format described as follows).

#### `ping` packet

- Action: "ping"
- Data: null (ignored)

Ping request, server will respond with pong. Mainly used for testing. Unless the server is severely overloaded, the server will respond with a "pong" notification in no time.

#### `send_message` packet

- Action: "send_message"
- Data:

  ```json
  {
    "chat_id": int,   // The chat id to send the message to
    "content": str,   // The message content
    "reply_to": int   // Optional, the message id to reply to
  }
  ```

Send a message to a chat. You must be a member of the chat to send a message; the content must be a non-empty string.

If the reply_to field is set, the message will be a reply to the message with the specified id, where the replied message must be in the same chat.

If any error condition is met, the server will send an "error" notification with the error message; or a "new message" notification will be sent to all chat members.

After the message is sent, all messages in this chat is marked as read for the sender automatically.

#### `recall_message` packet

- Action: "recall_message"
- Data:

  ```json
  {
    "message_id": int,
    "delete": true
  }
  ```

Recall a sent message or delete a message.

If delete==true, the message will be deleted, only for the current user; else the message will be recalled (deleted for all users).

You will receive a "message deleted" notification if the message is deleted; or a "message recalled" notification if the message is recalled; otherwise, an "error" notification will be sent.

You must be the sender to recall the message.

#### `messages_read` packet

- Action: "messages_read"
- Data:

  ```json
  {
    "chat_id": int
  }
  ```

Mark all messages of a certain chat as read.

If the user is in the chat, all messages in the chat will be marked as read and all members in the group will receive a "messages read" notification; otherwise, an error response will be sent.

### Server to Client packets:

#### Error packets

If a client-side error occurs, the server will return an error packet with the following format:

```json
{
  "action": "error",
  "request_id": 0,
  "ok": false,
  "code": 400,
  "error": "ERROR_MESSAGE"
}
```

Where "action" is set to "error", "ok" is set to false, "code" is currently not defined (400 for most cases),
and "error" is the error message. "request_id" is set to the request_id of the packet that caused the error, or 0
if the request_id is not sent / cannot be determined.

#### Notification packets

Notification packets are actively sent to the client to notify the client of certain events. The format is as follows:

```json
{
    "action": "ACTION_NAME",
    "request_id": 0,
    "ok": true,
    "data": any
}
```

#### `logout` notification

- Action: "logout"
- Data: null
- Target: Current session

Notify the client that the user has been logged out. The connection will be actively closed after this notification. The frontend should redirect the user to the login page.

#### `profile_change` notification

- Action: "profile_change"
- Data: null
- Target: Current user

Notify the client that the user's profile has been changed. The client should update the user's profile information.

#### `new_group_chat` notification

- Action: "new_group_chat"
- Data:

  ```json
  {
    "chat_id": int
  }
  ```

- Target: All users in chat

Notify the client that a new group chat has been created.

#### `new_message` notification

- Action: "new_message"
- Data:

  ```json
  {
    "message": `ChatMessage`
  }
  ```

- Target: All users in chat

Notify the client that a new message has been sent to a chat.

#### `message_deleted` notification

- Action: "message_deleted"
- Data:

  ```json
  {
    "message_id": int
  }
  ```

- Target: Current user

Notify the client that a message has been deleted.

#### `message_recalled` notification

- Action: "message_recalled"
- Data:

  ```json
  {
    "message_id": int
  }
  ```

- Target: All users in chat

Notify the client that a message has been recalled.

#### `messages_read` notification

- Action: "messages_read"
- Data:

  ```json
  {
    "chat_id": int,
    "user_id": int
  }
  ```

- Target: All users in chat

Notify the client that a user has read all messages in a chat.

#### `admin_state_change` notification

- Action: "admin_state_change"
- Data:

  ```json
  {
    "chat_id": int,
    "user_id": int,
    "is_admin": bool
  }
  ```

- Target: All users in chat

Notify the client that a user's admin status has been changed.

#### `owner_state_change` notification

- Action: "owner_state_change"
- Data:

  ```json
  {
    "chat_id": int,
    "owner_id": int
  }
  ```

- Target: All users in chat

Notify the client that the chat owner has been changed.

#### `member_deleted` notification

- Action: "member_deleted"
- Data:

  ```json
  {
    "chat_id": int,
    "user_id": int
  }
  ```

- Target: All users in chat (including the user being removed)

Notify the client that a user has been removed from a chat.

#### `member_added` notification

- Action: "member_added"
- Data:

  ```json
  {
    "chat_id": int,
    "user_id": int
  }
  ```

- Target: All users in chat (including the user being added)

Notify the client that a user has been added to a chat.

#### `chat_deleted` notification

- Action: "chat_deleted"
- Data:

  ```json
  {
    "chat": `Chat`
  }
  ```

- Target: All users in chat

Notify the client that a chat has been deleted.

#### `chat_invitation` notification

- Action: "chat_invitation"
- Data:

  ```json
  {
    "invitation": `ChatInvitation`
  }
  ```

- Target: Chat owner and admins

Notify the client that a new chat invitation has been sent.

#### `friend_deleted` notification

- Action: "friend_deleted"
- Data:

  ```json
  {
    "friend": `User.Detailed`
  }
  ```

- Target: The friend being deleted (the opposite user, Friend.friend)

Notify the client that a friend has been deleted.

#### `friend_created` notification

- Action: "friend_created"
- Data:

  ```json
  {
    "friend": `User.Detailed`
  }
  ```

- Target: Both users

Notify the client that a new friend has been created.
