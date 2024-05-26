## Modules in Frontend

### Structure and Explanation for Source Files

```tree
.
├── Dockerfile
├── README.md
├── nginx
│   └── frontend.conf
├── package-lock.json
├── package.json
├── postcss.config.js
├── sonar-project.properties
├── src
│   ├── App.css Generate by create-react-app.
│   ├── App.tsx General framework entry point of the APP.
│   ├── chat_control Related to chat rooms and messages.
│   │   ├── components
│   │   │   ├── ApplicationsForChatList.tsx Show applications for a chat if the user is admin or higher.
│   │   │   ├── InviteFriendIntoChat.tsx Component to select friends to invite into chat.
│   │   │   ├── MessageAssociateInfo.tsx Display send time and read count or read by whom.
│   │   │   ├── MessageContent.tsx Message content container.
│   │   │   ├── MessageTab.tsx Each for a message separately. Then calls others like MessageContent.
│   │   │   ├── MessagesFilterContainer.tsx Filter messages by time and sender.
│   │   │   ├── NoticeCard.tsxShow a single notice in a group chat.
│   │   │   ├── NoticesBar.tsx Show notices in a group chat beneath the header.
│   │   │   ├── RepliedMessageTab.tsx Display the replied message.
│   │   │   ├── SIngleUserTab.tsx Display members info in MoreOfChat page.
│   │   │   ├── SingleChatTab.tsx Each tab in the chats list.
│   │   │   └── UserTabTemplate.tsx For multiselect component to show each user.
│   │   ├── createGroupChat.ts Create a group chat api.
│   │   ├── deleteChat.ts Delete a chat api.
│   │   ├── filterMessages.ts Filter messages by time and sender.
│   │   ├── getChatInfo.ts Get the chat info api.
│   │   ├── getChats.ts Get all the chats api.
│   │   ├── getDetailedMessages.ts Get all the messages in a chat room.
│   │   ├── getDetailedMessagesVerbosely.ts Get all the messages in a chat room with verbose return value.
│   │   ├── inviteToGroupChat.ts Invite a friend to a group chat api.
│   │   ├── kickoutMember.ts Kick out a member from a group chat api.
│   │   ├── leaveChat.ts Leave a chat api.
│   │   ├── listApplicationsForAllChats.ts List all the applications for all chats.
│   │   ├── listApplicationsForChat.ts List all the applications for a chat api.
│   │   ├── pages
│   │   │   ├── ChatHeader.tsx The header of the chat room.
│   │   │   ├── ChatMainPageFramework.tsx The whole chat module framework including all chats and single chat.
│   │   │   ├── ChatSideBar.tsx Display chats list in the left side.
│   │   │   ├── CreateGroupChat.tsx Create a group chat using friends.
│   │   │   ├── DialogBox.tsx Display the bottom dialog box to enter messages.
│   │   │   ├── Dialogs.tsx Display all messages in a chat room.
│   │   │   ├── MoreOfChat.tsx Display the chat management page and members. 
│   │   │   ├── SingleChatMain.tsx The container for header, dialogs and dialog box of one chat.
│   │   │   └── css
│   │   │       └── auto-hidden-scroll.css
│   │   ├── respondToApplicationForChat.ts
│   │   ├── setAdmin.ts
│   │   ├── states
│   │   │   ├── CurrentChatProvider.tsx
│   │   │   ├── DialogBoxRefProvider.tsx
│   │   │   ├── MessageRefsProvider.tsx
│   │   │   ├── RefetchProvider.tsx
│   │   │   ├── RepliedMessageProvider.tsx
│   │   │   └── updateChatState.ts
│   │   ├── transferOwner.ts
│   │   ├── ui
│   │   │   └── MessageTabListItem.ts
│   │   └── utils
│   │       ├── getInvitableFriends.ts
│   │       ├── getIsSelf.ts
│   │       └── parseSystemMessage.ts
│   ├── friend_control Related to managing friends relations and groups.
│   │   ├── DeleteFriendButton.tsx Click and delete friend.
│   │   ├── FriendsForEachGroupList.tsx Friends list in a single group.
│   │   ├── FriendsList.tsx Friends list that will sort by groups and use the above to display.
│   │   ├── FriendsPage.tsx Load friends and groups and call the above to display.
│   │   ├── GroupSetting.tsx Setting component for each group.
│   │   ├── InviteFriendPage.tsx Component that will pop up to write comments and invite a friend.
│   │   ├── OngoingInvitations.tsx Display the friendship invitations.
│   │   ├── SearchNewFriend.tsx Page for searching others.
│   │   ├── SearchNewFriendResultList.tsx Display the search results.
│   │   ├── SingleFriendSetting.tsx Setting component for each friend, for example, belonged group.
│   │   ├── UserDisplayTab.tsx Display the user info in the friends list.
│   │   ├── acceptInvitation.ts Accept the invitation api.
│   │   ├── createGroup.ts Create a group api.
│   │   ├── deleteFriend.ts Delete a friend api.
│   │   ├── deleteGroup.ts Delete a group api.
│   │   ├── editGroupName.ts Edit a group name api.
│   │   ├── getDefaultGroup.ts Get the default group by checking whether name is empty string.
│   │   ├── getFriendInfo.ts Get the friend info api.
│   │   ├── getFriendsList.ts Get the friends list api.
│   │   ├── getGroupsList.ts Get the groups list api.
│   │   ├── getInvitations.ts Get the pending invitations api.
│   │   ├── invite.ts Invite a friend api.
│   │   ├── queryGroup.ts Query a group info api.
│   │   ├── rejectInvitation.ts Reject the invitation api.
│   │   ├── searchFriend.ts Search users api.
│   │   ├── updateFriend.ts Update the friend setting api.
│   │   └── utils Get appropriate name for any user.
│   │       ├── parseAnyoneName.ts
│   │       ├── parseDisplayName.ts
│   │       └── parseNameOfFirend.ts
│   ├── index.css Generate by create-react-app.
│   ├── index.tsx Router definition and the entry point of the APP.
│   ├── main_page
│   │   ├── MainPageFramework.tsx The main framework that users see except for login page.
│   │   └── SideBarLink.tsx Left-side links used in MainPageFramework.
│   ├── svg Svg images.
│   │   ├── cancel-svgrepo-com.svg
│   │   ├── fold-down-svgrepo-com.svg
│   │   ├── fold-up-svgrepo-com.svg
│   │   ├── more-horizontal-square-svgrepo-com.svg
│   │   ├── nav-chat-icon.svg
│   │   ├── nav-friend-icon.svg
│   │   ├── nav-group-icon.svg
│   │   ├── nav-invitation-icon.svg
│   │   ├── nav-search-icon.svg
│   │   ├── nav-setting-icon.svg
│   │   └── nav-upcoming-icon.svg
│   ├── user_control Related to the current user herself or himself.
│   │   ├── AccountManagement.tsx User settings page.
│   │   ├── DisplayCurrentUserInfo.tsx Display user in the top left corner.
│   │   ├── Login.tsx Login page.
│   │   ├── RouterGuard.tsx Prevent user from accessing illegal pages.
│   │   ├── components Except for the first one, all for editing user info.
│   │   │   ├── DetailedInfoPopup.tsx Display detailed user info when click avatar.
│   │   │   ├── DialogFormSubmitButton.tsx
│   │   │   ├── DialogOldPasswordInput.tsx
│   │   │   └── EditDialog.tsx
│   │   ├── deleteAccount.ts Delete user account api.
│   │   ├── editUserInfo.ts Edit user info api.
│   │   ├── getUserInfo.ts Get current user info api.
│   │   ├── handleSubmittedLoginInfo.ts Handle login or register.
│   │   ├── isValidPath.ts Check if the path is valid in router guard.
│   │   ├── logout.ts Logout api.
│   │   └── utils
│   │       ├── userNameFormOptions.ts Some consts for the form for editting user_name.
│   │       └── validateUserName.ts Check extra rules for user_name.
│   ├── utils
│   │   ├── Asserts.ts Type assertions for typescript.
│   │   ├── AssertsForRouterLoader.ts Type assertions for Loaders.ts.
│   │   ├── ErrorPage.tsx Router error page.
│   │   ├── Loaders.ts Data loaders.
│   │   ├── TestsData.ts Data built for testing.
│   │   ├── Types.ts General types used in the APP.
│   │   ├── ValidationError.tsx Validation error component in forms.
│   │   ├── consts General consts used in the APP.
│   │   │   ├── DebugAndDevConsts.ts
│   │   │   ├── InputRestrictions.ts
│   │   │   └── SystemValues.ts
│   │   ├── parseChatName.ts Parse the chat name that should be displayed.
│   │   ├── router
│   │   │   ├── RouteParamsHooks.ts Conveninetly get the params from the URL.
│   │   │   └── RouterPaths.ts Router (url) paths as consts.
│   │   ├── time Deal with time related utility features.
│   │   │   ├── convertDateToUnixTImeStamp.ts
│   │   │   ├── unixTimestampToClockString.ts
│   │   │   ├── unixTimestampToDateString.ts
│   │   │   └── unixTimestampToExactTimeString.ts
│   │   └── ui Common UI features and components.
│   │       ├── Avatar.tsx
│   │       ├── TailwindConsts.ts
│   │       └── themes.ts
│   └── websockets
│       ├── Actions.ts All the websocket actions used.
│       ├── AssertsWS.ts Type assertions for websocket messages.
│       ├── WSTypes.ts Data type of websocket messages.
│       └── component
│           └── UpdateDataCompanion.tsx Deal with almost all the operations on receiving data.
├── tailwind.config.js
└── tsconfig.json
```