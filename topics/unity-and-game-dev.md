# Unity and Game Dev

## Unity

* What is a game engine: Think Xcode for building games. Gives you everything necessary to build a game without worrying about how it renders and builds. It does **not** provide your final artwork and assets. It simply allows you to tie them tall together with your code.
* Keep scenes in an **Assets/\_Scenes** folder.
* A prefab is a game object that stores several assets together \(3d models, materials, animations, etc\).
* **Project Setup**
  * Each frame in a game render as they are needed. You want to aim for a render speed of 60fps or higher. Window -&gt; Lighting -&gt; Settings. Uncheck "Realtime Global Illumination" and "Baked Global Illumination". Also uncheck "Auto Generate" at the bottom. Then close the window. \(Look up what these do later on.\)
  * Edit -&gt; Project Settings -&gt; Player. In the "Other Settings" section of the window, change the "Color Space" to "Linear".
  * Edit -&gt; Project Settings -&gt; Quality. Delete all except "Fastest" and "Fantastic". Rename "Fastest" to "Low Quality". Change the texture quality to "Quarter Res". Rename "Fantastic" to "High Quality".
  * Drag objects/prefabs from the project window to the hierarchy window to place them in the scene. Press F to center around the object.
* **Camera**

  * Place the camera in the scene by clicking on the "Game" tab at the top. Then click on the "Main Camera" in the Hierarchy window. Type the [position you want](https://teamtreehouse.com/library/position-the-camera) the starting point camera to be in in the Inspector window's "Transform" section. Next change the clipping planes to be what your game needs. Then you write the code for the camera to follow the player.
  * Simple camera affects: Asset Store -&gt; Post Processing Stack -&gt; Import. Main Camera -&gt; Add Component -&gt; Post Processing Behavior. Prefabs -&gt; Right Click -&gt; Create -&gt; Post Processing Profile -&gt; Name it. Drag it into the main camera's created component. Select the profile in the project window. Play with options.

* **Characters**

  * Pathfinding
    * Add Component -&gt; Navigation -&gt; Nav Mesh Agent. Go to the Scene tab and press F to see the enemy. Now change the Height and Radius to be the size of the character. And the speed \(how fast the character is\), and the angular speed \(how fast they turn\). Set the stopping distance to the appropriate level.
    * Click on the environment object. Now create a navmesh to the environment so that the characters know how to navigate around. Window -&gt; Navigation -&gt; Bake. Change the Agent Radius and Agent Height to be the average of your character's heights and radiuses.

* **Sound**

  * **Background Music**: Hierarchy Window -&gt; Create -&gt; Create Empty -&gt; Name it "2D Sound" since it won't be positional sound, simply flat music in the background. Then - with the 2D Sound object selected - Create -&gt; Create Empty Child. Name it "Music". Do it again and name the second child "Background". Now Add Component -&gt; Audio -&gt; Audio Source. Find the audio sources for the two game objects.  Make sure "Play on Awake" \(start when the game starts running, even if before the user presses play\) and "Loop" are checked.
  * **Sound Effects**: Create an empty child object on the object that makes the sound. Add an audio source component. Uncheck "Play on Awake". Slide "Spacial Blend" all the way up \(makes it a 3D sound\). In the "3D Sound Settings" section, change the minimum distance to about a third of the main camera's "Field of View" setting. If you're under that distance from the source, then you'll hear the sound perfectly clear. Now change the max distance to about twice the Field of View setting. If you're over this distance, you won't hear the sound at all. Anything between the 2 distances makes the sound decrease. These numbers are just rules of thumb, change them as the sounds would change in real life \(gun fire would have a much greater max distance than footsteps\). Be sure to disable any random sound players by clicking the checkbox next to the script so that it doesn't start until appropriate.
  * **Mixer**: Window -&gt; Audio Mixer. Move the Mixer to the **Sound **folder. You can create a group for sounds that should have identical mixing, or create a new group for each sound. Then click on the component with the sound component, click the circle next to the "Output" field, and double click on the group that it should output to. A neat thing to do is disable any enemies \(by clicking the checkbox next to the game object name in the inspector window\), or disable the ability to die, then in the mixer window , while the game is playing, click "Edit in Play Mode".

* **UI Overlay**

  * Create -&gt; UI -&gt; Text.
  * To anchor it, click the position square \(on the actual object, not the parent\) -&gt; hold alt -&gt; click where you want it to anchor. Then click the alignment button that corresponds with where you want the object in the **Paragraph** section of the Inspector window.

* **Scripts**

  * Select the object that the script will control in the Hierarchy Window. Add Component -&gt; New Script -&gt; Name the script \(one word, camel case with capital first letter, as it will be the class name\) -&gt; hit enter. Make sure to move the script to the **Scripts** folder \(Unity stores it in the main **Assets** folder by default.\)
  * After saving the script, also hit Cmd + S in Unity to save the scene.
  * You can add the same script to multiple objects.

* **UnityEngine**

  * **Input.GetAxisRaw\(\)**: Takes a string either `"Vertical"` or `"Horizontal"` and returns a float from **-1** to **1** stating whether the user is trying to move in the current frame.
  * **void FixedUpdate\(\)**: Used to update animator components that rely on physics. This happens after the Update method, but not as often.

* **Building**

  * File -&gt; Build Settings -&gt; Choose platform -&gt; Add Current -&gt; Choose Target Platform -&gt; Change Architecture to 86 \(which will allow it to be played on the lowers as well\) -&gt; Built -&gt; Select or create the "Builds" folder -&gt; Name and save. You have a game!
  * If you build for Windows/PC, the executable will have a appname\_data folder with it as well. This folder has the assets, and should remain in the same folder as the .exe file. So if you're trying to share the game, zip them up and send the zip file.

* **Tips**

  * After adding any assets with their own lighting, go to Window -&gt; Lighting -&gt; Settings and click "Generate Lighting".
  * Hotkeys: Alt click+drag. F, Q, W, E, R, Z, and X.
  * Use the Asset Store for things that may come in handy.
  * Lock the inspector widnow by clicking the little lock icon in the top right. This way you can click multiple things in the project window for dragging without the inspector window changing.
  * You can duplicate component settings by clicking the gear icon and clicking "Copy Component", then in another game object clicking "Past Component Values". You can also paste a completely new component if you don't have it yet with "Past Component as New".
  * The Y axis in `Verctor3` is up, like a jump. Not up like a vertical movement.
  * Manipulate the RigidBody of the object, not the transform itself. RigidBody is affected by physics, so if some other force is acting on the object at the same time, RigidBody will intelligently calculate where the object should be. Transform will not do that.
  * You can expose class properties to the inspector window. This is helpful for locking one object's value to the value of another object's property. \(E.g: The playerTransform property in the camera object to the actual player object's transform value\).
  * Every game object in unity stores a reference to itself in `gameObject`.
  * When creating game objects that aren't attached to a particular asset, always hit the gear icon in the Inspector Window, then "Reset".

## Process

1. **Ideation**
   1. Satisfy this requirement first: "It turns out that it boils down to a kind of pragmatic freedom to roam coupled with an odd combination of social factors. The fundamental human desire to master an environment in concert with the stories that these games generate for the player seems to be utterly compelling, independent of any idea of skill, progression … or even the game being actually finished and fully playable. You could lose years trying to figure this out fully."
   2. Next come up with new and interesting characters that can be latched on to \(and easily promoted\). With full backstories. \("You should know how much change is in their pockets at any given time."\). 
   3. Then come up with an amazing story arc and backdrop. \(Feel free to use inspiration from sources we've screenshot and saved\).
   4. How will you be marketing the game? What's the marketing budget? IG ads? Other ads? PR? Influencers? Reddit mods of /r/gaming? What percentage of revenue from the game will go back into marketing? For every 1 person that plays the game, 3 more should be spawned. So if the game is super viral \(each person is telling 3 friends\), then no ads are necessary. If not, then hopefully the game is sticky enough that the revenue from 1 person playing pays for acquiring 3 new players plus profit.
   5. Finally solidify your revenue streams for this particular game. Ads only? IAP to get rid of ads? Other IAPs?
2. **Production**
   1. **Design**: 
      1. Deciding _how_ the game should play and feel \(What are the perfect settings and platforms for the game? If it's mobile it should pass the toilet test **and** the quick show off test. If console then the Saturday on my own test \(Jak and Daxter, Uncharted\) **or** the party/everyone will have fun test \(Wii Sports, Rocket League\). 2D vs 3D? Sound effects inspirations. Soundrack inspirations. Overall mood? Physics inspirations.\) 
      2. Upon first playing the game, how are your subjects gifted with **surprise and delight**. How is the game both **new** and **recognizable** at the same time? \(Ex: Rocket league is soccer, but with cars. Flappy birds had a Mario feel, but it was a bird.\)
      3. What are the set of problems that your subject has to solve? 
      4. How do these problems vary in design and difficulty \(levels\)? 
      5. What are the risks and rewards that they have to balance at any given moment?
      6. What makes this game **remarkable**? Are there inherent viral attributes? Is there something particurlaly different? Why will this game spread like wildfire?
      7. How will you build strong IP? \(Strong, recognizable characters and symbols\). How will you profit off of the IP? Should you work with a publicist to treat the IP like a celebrity? Licensing deals? Selling merch? Daily vlog? 
   2. **Art & Assets**: 
      1. **3D Models**
         1. First look to the Asset Store for anything you may need that fits the bill.
         2. Characters should have both an idle animation, and a movement animation component for when the user is actively moving.
         3. Idle Animation
         4. Make sure that props are checked as "Static" \(meaning that they will not be moving\) so that unity can perform its optimizations for them. The checkbox is in the inspector window next to the naming field.
      2. **Textures**
      3. **Level UI**
      4. **Scripts**
      5. **Sound Effects**
      6. **Music**
   3. **Development**
      1. First import all assets/prefabs and setup the initial assets of the game \(environment/props, player, camera \(FPS, following player, etc\), and camera effects for the scene.
      2. **Pickups**: 
         1. Idle movement, different from idle animation \(fly moving in a circle\)
         2. Pickup animation/particles
         3. Pickup spawning: Need the object itself that needs to be spawned, minimum and maximum number of objects that should be spawned at any given time, total number that are currently spawned, and the area that they should be spawned. Make sure the items can never spawn within a certain radius of the player.
      3. **Other Characters**
         1. Pathfinding: 
            1. Collisions can happen multiple times very fast, so it's best to include something in the if condition that will make sure it only happens once \(like something that changes inside of the block\) if the colliding object isn't destroyed by the collision.
      4. **Game State**
         1. Before the user manually starts the game, no characters should be moving.
         2. Overlay: Use Unity's UI overlay features. Check "Pixel Perfect" in the **Canvas** object to make the letters more crisp. For the **Text** object, always set the height and width slightly larger than the perferred sizes in the "Layout Properties" window at the bottom right after you change the font size.
         3. Health: Of course, the player health should be a public property.
      5. **Audio**
         1. Make sure you have a script for the audio to not destroy every time a new scene loads.
         2. Each sound should be as realistic as possible in its depth and frequency with the exception of background music and success effects \(pickups, kills, etc\), which should be 2D.
         3. Mixing is just as important. Make sure the sounds are at the right levels.
      6. **Monetization and Analytics**
      7. **Secondary Assets**
         1. Gameplay videos
         2. Images \(Screenshots, Gifs, Logos\)
         3. Website
   4. **QA and Playtesting**
   5. **Build**
3. **Publish**
   1. Publishers: Developer Digital, Paradox, Surprise Attack, tinyBuild, Indie Fund, Team 17, Adult Swim Games, Finji, Devolver, Curve, Double Fine
   2. Networking with publishers: You need to ask them exactly, "What is the checklist for you giving a green light to a game?" And work on giving them exactly what they want.
   3. What are they going to do for you? What do you want done? Do these match?
   4. Eventually you want to erase these step by step and keep a larger piece of the pie. So first you're looking for funding for all three, then just marketing and distribution, and finally just distribution \(keeping 85% of the money\). \(150M in company stock, 5M in overhead for 5 years, 25M to finance acquisitions for the business, 10M for each project for a minimum of three\).
   5. **Production** **Funding**: How much upfront \(enough to last me until the game launches plus 3 months after, plus profit\)? Money usage rules? How much on the backend and at what point? How does that backend money flow? Who cuts the checks? In what increments?
   6. **Marketing** \[PR, Advertising, Events\]: Who exactly runs the PR? Is it just North America? Do they include streamers, Youtubers, and other social media? How are they spending advertising money, and where? What events do they go to and what happens there? Is their cohesion in their campaigning? Can they build excitement toward a release date
   7. **Distribution** \[digital, retail, merch, direct\]: Placement on storefronts? Inclusion in promotions, merchandising? Cross promotion?
   8. Who have they worked with before? How did it go? \(Go talk to them, and the ones that didn't sell many copies\)
   9. Do they believe in and are they excited about the game?
   10. In their past work, does the game have more visibility than the publisher's name?
   11. IP: IP is under no circumstances up for grabs. Owned by the developer's studio.
4. **Squeeze** as much as you can out of it.
5. **Sell**: Maple Media



