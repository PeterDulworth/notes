# SERVER- Final Project User Manual

**Authors:** Peter Dulworth (psd2), Sophia Jefferson (sgj1) 

**Group: **26

### 1. Starting Up

To Start the server run the `FinalProjectServer_PortSet1.launch` file.

### 2. Connecting To Other Users

There are two ways to connect to another user:

1. In the discovery server window enter a category and press the `Connect` button. Select and endpoint that you would like to connect to and press `Get Selected Endpoint`. 
2. Enter the IP address of the user you would like to connect to in the `Enter IP Address` text box. Press the `Connect` button directly below it.

After doing either of these ther user name of the newly connected user will appear in the `Connected Users` drop down on the main GUI.

### 3. Using the Program - Basic Functionality

**Creating Chatrooms**

1. Enter a name in the `Enter Chatroom Name` text field. 
2. Press the `Create` button directly below it.

**Inviting Users To Chatrooms**

After you have connected to a user using the steps above, you can invite them to a chatroom that you are in. 

1. Select the user you wish to invite in the `Connected User` drop down.
2. Select the room you wish to invite them to in the `Connected` drop down. 
3. Press the `Invite button` to the right of the `Connected` drop down to invite them. 

If successful the chat room should print a message saying who was added.

**Joining Chatrooms**

If you wish to join a chatroom of a user that you are connected to without being invited:

1. Select the `Available` drop down list. This will refresh it. 
2. Select the room you wish to join from that drop down list and press the `Join` button.

**Refreshing a Chatroom Memberlist**

In a chatroom you can press the `Refresh Members` button to get the current list of members.

**Sending a Chatroom Text Message**

In a chatroom enter a message in the bottom text field and press enter.

**Sending an Image to a Chatroom**

1. Press `Send Image`.
2. Select an an image in the modal that pops up.
3. Press `Open`.

**Leave a Chatroom**

To leave a chatroom, press the `Leave` on the top left of the chat room panel.

### 4.Using the Program - Game Functionality

#### Serving a Game

Serving a game has three main steps.

1. Create or join a chatroom (as described above) with all the members that wish to play the game. This chatroom will serve as the `Lobby`.

2. From the `Lobby` chatroom, press the `Create Teams` button. This will add two new chat rooms (as tabs) to the server: `Team 1` and `Team 2`. It will also split the lobby in half and invite half to each team. You can check which players are on which team by pressing the `Refresh Members` button on each team chat room.
3. From the `Lobby` chatroom, press the `Install Game Cmds` button. This will load all the clients with everything they need to play the game.
4. From the `Lobby` chatroom, press the `Send Game!` button.

### 5. Example Game Creation Flow

1. Connect to all the users you want to send your game to.
2. Great a new chat room `Game Lobby` and invite all the users to it.
3. From the `Game Lobby` press `Create Team`.
4. From the `Game Lobby` press `Install Game Cmds`.
5. From the `Game Lobby` press `Send Game!`.

### 6. Quitting the Program

To quit the program, simply click the `Quit` button. This will automatically leave all chatrooms you have joined, disconnect from all users you have connected to, and quit the application.

