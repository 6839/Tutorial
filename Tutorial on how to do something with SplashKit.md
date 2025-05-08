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



# Music class practice

## 1. Scenario-based music transition effects

In games or multimedia applications, smooth transition of music is realized according to scene switching. For example, when the player enters the battle area from the safe area, the music of the old scene gradually fades to the end (or connects with the new music), and the new battle music starts to play with a fading effect, which enhances the immersion of the atmosphere.

```
// Assume that there are Music instances for the old music oldMusic and the new music newMusic.
// oldMusic plays a few times before fading in and out (or depending on the logic)
oldMusic.Play(1); 
newMusic.FadeIn(2000); // new music fades in within 2 seconds
```

## 2. Rhythmic music interaction

In interactive applications such as music games or rhythmic interactive programs, multiple plays of music are triggered based on specific rhythms or events. For example, when a user completes a series of actions in a sequential manner, a music clip with a specific rhythm is played, and the experience is enhanced by controlling the number of times it is played and the fade-in effect.

```
// Assuming the user completes an action that triggers a music snippet to play
Music rhythmMusic = new Music("rhythm", "rhythm_snippet.mp3");
rhythmMusic.Play(1, 0.8); // Play once at 80% volume, can be called multiple times to overlay rhythms.
```

## 3. Multi-layered musical atmosphere creation

Play different types of music (e.g., ambient, background) in large scenes (e.g., open-world games) with multiple `Music` instances stacked on top of each other, and utilize fade-in and playback count controls to build rich audio hierarchies.

```
Music ambientMusic = new Music("ambient", "ambient_sound.mp3");
Music bgMusic = new Music("bg", "bg_music.mp3");

ambientMusic.Play(1, 0.3); // Play the ambient sound at 30% volume first
bgMusic.FadeIn(3000); // Background music fades in within 3 seconds, overlaid with ambient sounds
```

## 4. Story-driven music playback

In narrative applications (e.g., visual novels, interactive storytelling), the playback of music is controlled according to the development of the plot. For example, the climax of the plot repeats a rousing music clip and gradually increases the volume (by playing it multiple times and adjusting the logic).

```
Music climaxMusic = new Music("climax", "climax_music.mp3");
// Play the climax
for (int i = 0; i < 3; i++) // play 3 times to simulate the climax progression
{
    climaxMusic.Play(1, 0.5 + 0.1 * i); // increase volume each time it plays
}
```

## 5. Simple music queue playback

Achieve a music queue-like effect by manually managing multiple `Music` instances. Play different music in order, fading in before the start of each song.

```
List<Music> musicQueue = new List<Music>
{
    new Music("track1", "track1.mp3"),
    new Music("track2", "track2.mp3")
};

int currentTrack = 0;
void PlayNextTrack()
{
    if (currentTrack < musicQueue.Count)
    {
        musicQueue[currentTrack].FadeIn(1500); // Fade in to play current music
        currentTrack++;
    }
}
// Call PlayNextTrack() to trigger playback sequentially
```







# Timer class practice

## 1. Countdown feature in the game

Use the `Start` and `Ticks` methods of the `Timer` class to realize the countdown function in the game.

```
public class GameLevel
{
    private Timer countdownTimer;
    private int totalTimeInSeconds;
    private uint startTimeTicks;

    public GameLevel(int totalTime)
    {
        totalTimeInSeconds = totalTime;
        countdownTimer = new Timer("GameLevelTimer");
    }

    public void StartLevel()
    {
        countdownTimer.Start();
        startTimeTicks = countdownTimer.Ticks;
    }

    public int GetRemainingTime()
    {
        uint elapsedTicks = countdownTimer.Ticks - startTimeTicks;
        int elapsedSeconds = (int)(elapsedTicks / 1000); // assuming 1 second per 1000 ticks
        return totalTimeInSeconds - elapsedSeconds;
    }

    public bool IsTimeUp()
    {
        return GetRemainingTime() <= 0;
    }
}
```

## 2. Scheduled task scheduler

Utilize the `Start` and `Ticks` methods of the `Timer` class, and combine them with a loop to achieve timed task scheduling.

```
using System;

public class TaskScheduler
{
    private Timer taskTimer;
    private Action task;
    private uint intervalTicks;
    private uint lastExecutionTicks;

    public TaskScheduler(Action taskToExecute, TimeSpan executionInterval)
    {
        task = taskToExecute;
        intervalTicks = (uint)executionInterval.TotalMilliseconds;
        taskTimer = new Timer("TaskSchedulerTimer");
    }

    public void StartScheduling()
    {
        taskTimer.Start();
        lastExecutionTicks = taskTimer.Ticks;
        while (true)
        {
            uint currentTicks = taskTimer.Ticks;
            if (currentTicks - lastExecutionTicks >= intervalTicks)
            {
                task();
                lastExecutionTicks = currentTicks;
            }
        }
    }

    public void StopScheduling()
    {
        taskTimer.Stop();
    }
}
```

## 3. Animation frame control

Use the `Start` and `Ticks` methods of the `Timer` class to control the switching of animation frames.

```
using System.Collections.Generic;

public class AnimationController
{
    private Timer frameTimer;
    private List<Bitmap> frames;
    private int currentFrameIndex;
    private uint frameIntervalTicks;
    private uint lastFrameChangeTicks;

    public AnimationController(List<Bitmap> animationFrames, TimeSpan frameDuration)
    {
        frames = animationFrames;
        frameIntervalTicks = (uint)frameDuration.TotalMilliseconds;
        frameTimer = new Timer("AnimationControllerTimer");
    }

    public void StartAnimation()
    {
        frameTimer.Start();
        lastFrameChangeTicks = frameTimer.Ticks;
    }

    public void StopAnimation()
    {
        frameTimer.Stop();
    }

    public void Update()
    {
        uint currentTicks = frameTimer.Ticks;
        if (currentTicks - lastFrameChangeTicks >= frameIntervalTicks)
        {
            currentFrameIndex = (currentFrameIndex + 1) % frames.Count;
            lastFrameChangeTicks = currentTicks;
            // update the animated display here
        }
    }
}
```

## 4. Performance testing and optimization

Use the `Start`, `Stop`, and `Ticks` methods of the `Timer` class to measure the execution time of code.

```
public class PerformanceTester
{
    private Timer performanceTimer;
    private uint startTimeTicks;
    private uint endTimeTicks;

    public void StartTest()
    {
        performanceTimer = new Timer("PerformanceTestTimer");
        performanceTimer.Start();
        startTimeTicks = performanceTimer.Ticks;
    }

    public TimeSpan EndTest()
    {
        performanceTimer.Stop();
        endTimeTicks = performanceTimer.Ticks;
        uint elapsedTicks = endTimeTicks - startTimeTicks;
        return TimeSpan.FromMilliseconds(elapsedTicks);
    }
}
```



# Bitmap class practice

## 1.**Customized drawing and editing tools**

### (1) **Vector graphics editor**

- **Functions**: Supports drawing circles, rectangles, lines, text and other elements, setting colors, line thickness, fill styles (e.g. gradient, transparency), and saving as bitmap.

- **Core methods**:
  - `DrawCircle`/`FillCircle`: draw hollow/solid circle.
  - `DrawRectangle`/`FillRectangle`: draw hollow/solid rectangle.
  - `DrawText`: render text at the specified position (supports font, font size and color configuration).
  - `Clear`: clear the canvas, support undo/redo.

- **Example Scenario**: Realization of a simple logo design tool where users can freely combine graphics and text.

### (2) **Dynamic chart generation**

- **Functions**: dynamically generate line charts, bar charts, pie charts, etc. based on data and export them as images.
- **Core Methods**:
- `DrawLine`: draw axes and data curves for line charts.
  - `FillRectangle`: draw bars for bar chart.
  - `FillEllipse`: draw sector of pie chart.
  
- **Example**: show real-time player data in the game (e.g. score trend, resource consumption).

## 2.**Sprites and collision systems in game development**

Combining bitmap cell (Cell) and collision detection methods to realize game sprite animation and physical interaction.

### (1) **Sprite animation and frame management**

- **Function**: Split a Sprite Sheet into multiple cells and play it frame by frame to form an animation (e.g. character walking, attacking movements).
- **Core method**:
- `SetCellDetails`: define the width, height and number of rows and columns of the cell to split the Sprite Sheet.
  - `CellRectangle`/`CellOffset`: get the cell position and offset, realize frame switching.

```
// initialize the cell: 32x32 pixels, 4 columns and 3 rows, 12 frames.
bitmap.SetCellDetails(32, 32, 4, 3, 12); 
// draw frame 5 to coordinates (100, 100)
bitmap.DrawBitmap(destinationBitmap, 100, 100, bitmap.RectangleOfCell(5)); 
```

### (2) **Accurate collision detection system**

- **Function**: detect collisions of bitmaps with circles, rectangles, and other bitmaps to realize game character interactions (e.g., picking up props, dodging obstacles).
- **Core Methods**:
  - `CircleCollision`: detect bitmap collision with circle (e.g. character with circle trap).
  - `BitmapCollision`: detect pixel-level collision between two bitmaps (e.g. bullet vs enemy).
  - `PointCollision`: detect if the click position is within the pixel area of the bitmap (implement UI button click detection).

- **Example Scenario**: In a 2D platform game, detect the collision between player character (bitmap) and gold coins (circle) to trigger the scoring logic.

## 3.**Interactive UI component development**

Use bitmap drawing and collision detection to build dynamically interactive UI elements.

### (1) **Customized buttons with tactile feedback**

- **Functionality**: draw buttons with visual feedback (change color/transparency when clicked), support mouse/touch events.
- **Core methods**:
  - `DrawBitmap`: draw the default and pressed state of the button.
  - `PointCollision`: detect if the mouse click position is inside the button area.

```
// Detect if the mouse click is on the button bitmap
if (buttonBitmap.PointCollision(mouseX, mouseY)) { 
    buttonBitmap.Draw(mouseX, mouseY, pressedStyle); // 绘制按下状态
    OnButtonClick(); // Trigger button event 
}
```

### (2) **Dynamic dashboard with progress bar**

- **Functionality**: real-time display of progress (e.g., life values, loading progress), dynamic filling by drawing rectangles or circles of different colors.
- **Core methods**:
  - `FillRectangle`: draw the filled area according to the progress value (e.g. green for healthy, red for dangerous).
  - `DrawText`: show progress value (e.g. 80%).

- **Example**: in-game life bar that gradually decreases fill length as the character is injured.

## 4.**Physics simulation with collision masks**

Complex physics interaction logic through `SetupCollisionMask` and boundary detection methods.

### (1) **Pixel-level collision masks**

- **Function**: Setup accurate collision mask for non-rectangular bitmap (e.g. irregularly shaped character) to avoid rectangular collision error.

- **Core method**:
- `SetupCollisionMask`: generate collision mask based on pixel data of the bitmap (automatically ignore transparent pixels).
  - `BoundingCircle`/`BoundingRectangle`: get the bounding box of the bitmap for fast collision detection.

- **Example**: Use pixel-level collision detection to avoid die-penetration problems when a character crosses a narrow passage in a role-playing game.

### (2) **Complex shape collision detection**

- **Function**: Detect bitmap collision with complex shapes such as triangles, quadrilaterals, rays, etc. (e.g. bullets with polygonal obstacles).

- **Core method**:
- `TriangleCollision`/`QuadCollision`/`RayCollision`: support collision detection with geometric shapes.
  
- **Example Scenario**: In a flight shooter game, detect the collision between a bullet (bitmap) and an enemy (triangle) to trigger damage calculation.



## 5.**Image processing and generation**

Dynamically generates or processes an image using bitmap drawing and manipulation methods.

### (1) **Dynamic texture generation**

- **Functions**: Generate random textures (e.g. grass, brick walls) at runtime, or draw customized patterns by code.

- **Core methods**:
  - `DrawPixel`: draw details pixel by pixel (e.g. noise textures).
  - `FillEllipse`/`DrawLine`: generate repeating geometric patterns (e.g. checkerboard grids, meshes).

- **Example**: Generate procedurally generated terrain textures.

### (2) **Validation code generator**.

- **Function**: dynamically generate verification code images containing random text, interference lines and noise to prevent automated attacks.

- **Core Methods**:
- `DrawText`: draw random string.
  - `DrawLine`: add interference lines.
- `DrawPixel`: add random noise.

```
for (int i = 0; i < 10; i++) {
    bitmap.DrawLine(randomColor, randomPoint, randomPoint); // drawing interference lines
}
bitmap.DrawText(randomString, randomColor, font, 20, 10, 10); // drawing verification code text
```

## 6.**Data visualization and information presentation**

Combining bitmap drawing and text rendering to realize complex information display interface.

### (1) **Interactive maps and annotations**

- **Function**: Dynamically add annotations (e.g. location names, route arrows) on the map bitmap, and support clicking on the annotations to display detailed information.

- **Core method**:
- `DrawBitmap`: load map background.
  - `DrawText`: draw location name (support different color and font size).
- `PointCollision`: detect if the clicked location is in the labeled area.
  
- **Example**: world map of the game, click on the city icon to show population, resources and other information.

### (2) **Dynamic logging and debugging panel**

- **Function**: Display debugging information (e.g. frame rate, memory usage) in real time in the game or application, support text highlighting and scrolling.

- **Core method**:
- `Clear`: clear the log area.
  - `DrawText`: draw log text by line (different colors to distinguish log level, e.g. red for error).

- **Example**: debugging tool in development phase, real-time display of player coordinates, enemy count and other data.



# Display class practice

## 1.**Responsive UI and resolution adaptation**

Realize dynamic layout through `Width` and `Height` to adapt to the resolution of different display devices.

### (1) **Adaptive UI scaling**

- **Function**: Automatically adjust the size and position of UI elements according to the display resolution to avoid stretching and distortion or black edges.

- **Core Logic**:
- Calculate the scaling ratio: `scale = min(target_resolution_width / Display.Width, target_resolution_height / Display.Height)`.
  - Scale the size and position of buttons, text, images and other elements proportionally.

```
float scaleX = (float)UI_TARGET_WIDTH / Display.Width;
float scaleY = (float)UI_TARGET_HEIGHT / Display.Height;
button.Position = new Point2D(button.Position.X * scaleX, button.Position.Y * scaleY);
```

### (2) **Immersive full-screen experience**

- **Function**: Detect whether the monitor is widescreen, ribbon screen or shaped screen (e.g. curved screen), and dynamically adjust the way the content is filled.

- **Core Methods**:
- Calculate the screen aspect ratio by `Width/Height` to adapt different ratio display area (e.g. 16:9, 21:9).
  - Crop or blur the content that exceeds the safe area (e.g. cell phone bangs screen adaptation).

- **Example scenario**:
- Automatically detect the monitor resolution when the game starts, switch to full-screen mode and hide the OS taskbar.
  - Video player automatically adjusts the video scaling mode (fill, keep ratio, stretch) according to the screen ratio.

## 2.**Display device information visualization and control**

Constructs an information panel or control center of a display device using properties such as `Name`, `Width`, and `Height`.

### (1) **System information monitoring interface**

- **Function**: Real-time display of current monitor's resolution, position, name and other information for debugging or system setting.

- **Core Methods**:
- Display monitor model (e.g. "Primary Display", "Monitor 2") via `Display.Name`.
  - Use `Bitmap.DrawText` to render resolution (`Width x Height`) and coordinates (`X, Y`) on the interface.

- **Example**:
- The debug panel in the developer tools displays detailed information about the monitor where the current window is located to assist in debugging the interface layout.

### (2) **Virtual resolution and display mode switching**

- **Function**: Supports user-defined virtual resolution, scaled proportionally to the physical display size during actual output.

- **Core Logic**:

  - User set the virtual resolution (e.g. 1920x1080), the program calculates the scaling factor based on `Display.Width` and `Height`.
- Renders the content at the virtual resolution and then stretches or centers it to the physical screen.
  
- **Example scenario**:

  - Retro style game: force rendering at 800x600 virtual resolution, add CRT scanline effect on edges to fit modern HD displays.

  

# Camera class practice

## 1. Character tracking in games

Enhance player immersion in a game by having the camera follow the character's movements to ensure that the character is always at the center of the screen or at a specific location.

```
// assuming the character sprite and window have been created
Sprite playerSprite = ...;
Window gameWindow = ...;

while (!gameWindow.CloseRequested)
{
    // make the camera always follow the character and the character is in the center of the screen.
    Camera.CenterOn(playerSprite, 0, 0);

    // render the game screen
    gameWindow.Clear(Color.White);
    // render other game elements
    gameWindow.Refresh();
}
```

## 2. Map exploration and zooming

Implement camera movement and zoom functions on the map to enable players to explore a larger game world.

```
// assuming a window and map have been created
Window gameWindow = ...;
Bitmap mapBitmap = ...;

// initial camera position
Camera.Position = new Point2D(0, 0);

while (!gameWindow.CloseRequested)
{
    // handle keyboard input to move the camera
    if (SplashKit.KeyDown(KeyCode.LeftKey))
    {
        Camera.MoveBy(-5, 0);
    }
    if (SplashKit.KeyDown(KeyCode.RightKey))
    {
        Camera.MoveBy(5, 0);
    }
    if (SplashKit.KeyDown(KeyCode.UpKey))
    {
        Camera.MoveBy(0, -5);
    }
    if (SplashKit.KeyDown(KeyCode.DownKey))
    {
        Camera.MoveBy(0, 5);
    }

    // render the map
    gameWindow.Clear(Color.White);
    gameWindow.DrawBitmap(mapBitmap, Camera.Position.X, Camera.Position.Y);
    gameWindow.Refresh();
}
```

## 3. Screen Shake Effect

Enhance the impact of the game by having the camera produce a vibration effect when events such as explosions and collisions occur.

```
// assuming the window and character sprites have been created
Window gameWindow = ...;
Sprite playerSprite = ...;

// initial camera position
Camera.Position = new Point2D(0, 0);

// shake parameters
int shakeDuration = 0;
int shakeIntensity = 10;

while (!gameWindow.CloseRequested)
{
    // handling the shake effect
    if (shakeDuration > 0)
    {
        Random random = new Random();
        double shakeX = random.Next(-shakeIntensity, shakeIntensity);
        double shakeY = random.Next(-shakeIntensity, shakeIntensity);
        Camera.MoveTo(Camera.Position.X + shakeX, Camera.Position.Y + shakeY);
        shakeDuration--;
    }
    else
    {
        // make the camera follow the player
        Camera.CenterOn(playerSprite, 0, 0);
    }

    // Simulate an explosion event to trigger the vibration
    if (SplashKit.KeyTyped(KeyCode.SpaceKey))
    {
        shakeDuration = 20;
    }

    // render the game screen
    gameWindow.Clear(Color.White);
    // render other game elements
    gameWindow.Refresh();
}
```

