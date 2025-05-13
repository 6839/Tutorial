# Brief Introduction

SplashKit is an open source, cross-platform multimedia library designed to simplify the development of games and graphics applications. It provides a range of easy-to-use APIs that support a variety of programming languages.

# Main Features

## 1. Cross Platform Compatibility

SplashKit can run on multiple operating systems, including Windows, macOS, Linux, and more. This means that developers can write code once and then deploy it on different platforms, greatly improving development efficiency.

## 2. Multi-language Support

It supports a variety of programming languages, such as Python, C++, C# and so on. Developers can choose the appropriate programming language for development according to their own preferences and project requirements.

## 3. Rich Multimedia Functions

- **Graphics Drawing**: Supports drawing all kinds of basic graphics, such as straight lines, rectangles, circles, ellipses, etc., as well as operations such as filling and drawing. It also supports loading and displaying image files, such as PNG, JPEG and so on.
- **Sound Processing**: It can play and manage various audio files, such as MP3, WAV, etc. It supports the loop playback of background music, the playback of sound effects and other functions.
- **Input Processing**: It can handle the events of input devices such as keyboard, mouse and gamepad, which is convenient for developers to realize interactive functions.

## 4. Easy to Learn and Use

SplashKit's API is designed to be clean and simple to understand and use. For beginners, it provides a low-barrier learning path to quickly get started with game and graphics application development.

## 5. Extensive Documentation and Examples

Officials have provided detailed documentation and a large amount of sample code to help developers better understand and use the features of SplashKit. These documents and examples cover a variety of common application scenarios, providing developers with a good reference.

# Tasks that can be accomplished

## 1. Game development

- **2D Games**: You can use SplashKit to develop various types of 2D games, such as platform games, shooting games, puzzle games and so on. You can create game characters, scenes and props through the graphic drawing function, use the input processing function to realize the player's operation control, and add game sound effects and background music with the help of the sound processing function.
- **Educational Games**: Due to its easy-to-learn and easy-to-use features, SplashKit is very suitable for developing educational games to help students learn programming and algorithms.

## 2. Graphics application development

- **Drawing tools**: Simple drawing tools can be developed to enable users to draw various graphs, add text and colors, etc.
- **Data visualization**: Data will be presented in graphical form, such as drawing bar charts, line graphs, pie charts, etc., to help users understand the data more intuitively.

## 3. Interactive application development

- **Demonstration programs**: create interactive demonstration programs that present information and effects through keyboard and mouse operations.
- **Simulation programs**: develop various simulation programs, such as physical simulation, biological simulation, etc., to help users better understand and study related fields.

# Networking class practice

## 1. **Multiplayer online game**

- **Function**: Multiplayer games (e.g. multiplayer shooting, racing or strategy games) that enable real-time synchronization between players.
- **Technology Implementation**:
  - Use UDP protocol to send player position, action data (e.g. moving, shooting).
  - Detect server messages through `HasMessages` to update local game status.
  - Example: Multiplayer space shooter with real-time synchronization of player ship position and bullet firing

```
// Opens a TCP connection to a server using the supplied details
private Connection _serverSocket;
_serverSocket = SplashKit.OpenConnection("GameServer", "127.0.0.1", (ushort)port, ConnectionType.TCP);

// Server-side: broadcast message to all clients
SplashKit.SendMessageTo(message, client);

// Processing Network Messages
while (SplashKit.HasMessages(_serverSocket))
{
	Message message = SplashKit.ReadMessage(_serverSocket);
	if(message != null)
	{
		// Example: Handling player movement messages
		string msgData = message.Data;
        if (msgData.StartsWith("PlayerMove"))
        {
            data = msgData.Substring("PlayerMove:".Length).Split(',');
            string playerId = data[0];
            float x = float.Parse(data[1]);
            float y = float.Parse(data[2]);

            if (_players.ContainsKey(playerId))
            {
                _players[playerId].UpdatePosition(x, y);
            }
        }
		SplashKit.CloseMessage(message);
	}
}
```



## 2. **Real-time collaboration applications**

- **Function**: Development of tools to support multi-person collaboration (e.g., real-time mapping, document editing).
- **Technology Implementation**:
  - Broadcast user actions (e.g. brush trails, text input) over UDP.
  - Use `UDPPacketSize` to optimize data transfer efficiency and avoid packet loss.
  - Example: Multiplayer online whiteboard where all participants can draw simultaneously.



## 3. **Distributed game server**

- **Function**: Build cross-server game architecture.

- **Technology Implementation**:

  - Exchange player data between servers via UDP (e.g. cross-server trading, teaming).
  - Use `HasMessages` for server load balancing and failover.
  - Example: In a massively multiplayer game, players move from one regional server to another.

  

## 4. **Real-time data visualization**

- **Function**: Development of dynamic visualization tools based on web data (e.g., real-time monitoring, sensor data presentation).

- **Technology Implementation**:

  - Receive data from external data sources (e.g. IoT devices, financial APIs) over the network.
  - Use SplashKit's graphical functions to render data changes in real time.
  - Example: Visualization map showing global seismic data in real time.

  

## 5. **Cloud archiving and synchronization**

- **Function**: cloud storage and cross-device synchronization of game progress.
- **Technology Implementation**:
  - Send game archive to server for saving via network.
  - Download the archive data when logging in on different devices.
  - Example: The progress of a role-playing game can be seamlessly switched between PC and cell phone.

```
// Server-side: save game archive
File.WriteAllText(filePath, saveData);

// Server-side: load game archive
File.ReadAllText(filePath);

// Client: upload archive
string message = $"Save:{playerId}:{saveData}";
SplashKit.SendMessageTo(message, _serverSocket);

// Client: download archive
string message = $"Load:{playerId}";
SplashKit.SendMessageTo(message, _serverSocket);
response = SplashKit.ReadMessage(_serverSocket);
data = response.Data.Split(':')[1];
```



# WebServer class practice

## 1. **Game web management interface**

- **Function**: Add a built-in web control panel to the game to monitor the game status in real time, modify parameters or perform administrative operations.
- **Technology Implementation**:
  - Use `WebServer` to listen on a specific port (e.g. 8080).
  - Handle HTTP requests and return HTML pages displaying game data (e.g. player list, server performance).
  - Receive administrative commands (e.g. kick, restart server) via form submissions.
  - Example: In a multiplayer game, an administrator accesses `http://localhost:8080/admin` through a browser to view online players and perform actions.

```
private string GenerateAdminPage()
{
	string playerList = "";
	foreach (var player in _gameServer.GetPlayers())
	{
		playerList += $"<tr><td>{player.Id}</td><td>{player.Name}</td>" +
					 $"<td><a href='/admin/kick?playerId={player.Id}'>踢人</a></td></tr>";
	}

	return $@"
		<html>
		<head><title>Game Control Panel</title></head>
		<body>
			<h1>Game Server Status</h1>
			<h2>Online Player ({_gameServer.PlayerCount})</h2>
			<table border='1'>
				<tr><th>ID</th><th>Name</th><th>Operation</th></tr>
				{playerList}
			</table>
			<h2>Servere Info</h2>
			<p>Running time: {_gameServer.UpTime}</p>
			<p>Memory Usage: {_gameServer.MemoryUsage} MB</p>
		</body>
		</html>";
}

    
if (request.Path == "/admin/kick")
{
	// Processing of kicking requests
	string playerId = request.QueryParameters["playerId"];
	_gameServer.KickPlayer(playerId);
	response.SetContent("Player has been kicked out", "text/plain");
}
```



## 2. **Web game frontend**

- **Function**: Combine SplashKit game logic with a web front-end to realize a browser-based game experience
- **Technology Implementation**:
  - Server-side processing of core game logic (e.g., physics simulation, AI).
  - Exchanges data with the client via WebSocket or polling mechanism.
  - The returned HTML page contains Canvas elements to render the game screen.
  - Example: Developing a multiplayer online strategy game where players connect to the game server through a browser.



## 3. **File servers**

- **Function**: Provide download service for game resources (e.g., images, audio, configuration files).
- **Technology Implementation**:
  - Handles HTTP GET requests and returns the contents of files with specified paths.
  - Support file caching and range request (breakpoint transfer).
  - Implement simple permission verification (such as the need to log in to download certain resources).
  - Example: Download the latest resource package from the server when the game client starts.

```
private string GenerateDirectoryListing(string dirPath, string urlPath)
{
	string items = "";
	foreach (var file in Directory.GetFiles(dirPath))
	{
		string fileName = Path.GetFileName(file);
		items += $"<li><a href='{urlPath}/{fileName}'>{fileName}</a></li>";
	}

	foreach (var dir in Directory.GetDirectories(dirPath))
	{
		string dirName = Path.GetFileName(dir);
		items += $"<li><a href='{urlPath}/{dirName}'>{dirName}/</a></li>";
	}

	return $@"
		<html>
		<head><title>Catalog Lists - {urlPath}</title></head>
		<body>
			<h1>Catalog: {urlPath}</h1>
			<ul>{items}</ul>
		</body>
		</html>";
}
```



## 4. **In-game shop**

- **Function**: Realize in-game virtual goods purchase system, support payment process.
- **Technology Implementation**:
  - Create product display page and shopping cart function.
  - Integrate payment gateways (e.g. PayPal).
  - Handle order confirmation and product release logic.
  - Example: Players purchase virtual items such as skins and props through the in-game store.



# Sprites related

**Sprites** in SplashKit are core game development components used to represent dynamic elements (e.g., characters, objects, effects, etc.) in a game. Sprites are movable, interactive graphical objects that support animation, collision detection, and physics simulation, enabling a wide range of functionality from simple animations to complex game systems.



## Core features

- **Animation System**: support multi-frame animation, can define different actions (e.g. walking, jumping, attacking).
- **Physical Attributes**: Physical parameters such as position, speed, acceleration, rotation, etc. can be set.
- **Collision Detection**: Built-in collision detection algorithm, support rectangle and circle collision body.
- **State Management**: You can switch different states of the sprite (e.g. life value, invincible state).
- **Layer Rendering**: supports layered rendering of sprites, control the display order.



## Interesting and complex feature implementation

### 1. **Multi-role action game**

- **Function**: Realize characters with complex action system, such as platform jumping, combos, and skill release.
- **Technology Implementation**:
  - Use sprite animation controller to manage different movements (e.g. running, jumping, attacking).
  - Gravity, collision and movement logic is handled by the physics engine.
  - Example: Character switches between different animation states based on user input and interacts with the environment.

```
// Create sprite
_sprite = SplashKit.CreateSprite("character");

// Create AnimationScript
_animationScript = SplashKit.AnimationScriptNamed("CharacterAnimations");

// Create Animation
Animation idleAnim = _animationScript.CreateAnimation("idle");
Animation runAnim = _animationScript.CreateAnimation("run");
Animation jumpAnim = _animationScript.CreateAnimation("jump");
```



### 2. **Particle systems and special effects**

- **Function**: Create visual special effects such as explosions, smoke, and magic effects.

- **Technology Implementation**:

  - Use sprites to generate tiny particles in batch, set different life cycle, speed and color.
  - Simulate natural movement (e.g. gravity, wind) through the physical properties of sprites.
  - Example: When a character releases a skill, generate a large number of glowing particles and move along a specific track.

  

### 3. **Physics simulation game**

- **Function**: Implement physics-based game mechanics, such as simulated vehicle driving and physics puzzles.

- **Technology Implementation**:

  - Set physical properties such as mass, elasticity, friction, etc. for objects.
  - Use SplashKit's physics engine to handle interactions between objects.
  - Example: Create a simulated pinball game where a ball collides with a baffle and creates a physical reaction.

  

### 4. **Dynamic weather system**

- **Functions**: Realization of environmental effects such as rain, snow, storms, day and night changes.
- **Technology Implementation**:
  - Use sprites to generate rain and snow particles in batch and set different motion tracks.
  - Dynamically adjust the lighting and color tone of the scene to simulate time changes.
  - Example: When it rains, generate a large number of falling raindrop sprites on the screen and reduce the overall brightness.

