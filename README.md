# webxr-aframe-immersive-football-sample
A sample immersive football website for Oculus Quest. 

Did you know that you can create immersive VR experiences for Oculus Quest without having to learn Unity/Unreal or needing to register for an Oculus Developer account? Immersive VR experiences have been touted as the future for a couple of years now, but the majority of companies have been hesitant to invest heavily in this new medium until there was a large enough market of VR users. This was understandable, given the cost of recruiting developers with the right skills and the creation of 3D assets could for some feel like a risky investment. And then you'd also have to deal with the practicalities of adapting your codebase for each different VR headset, not to mention getting approved to appear in their different app stores. 

Nowadays however, you can build immersive VR experiences for a fraction of the cost required in the past. And considering there are over [10 million units of Oculus Quest devices sold](https://www.theverge.com/2021/11/16/22785469/meta-oculus-quest-2-10-million-units-sold-qualcomm-xr2), it is much more likely you can earn a decent return on investment. By using [WebXR](https://immersiveweb.dev) and libraries such as [A-Frame](https://aframe.io), developing immersive 3D experiences is now almost as simple as creating a simple website, and these experiences can run directly on Oculus Quest and other VR devices with a single codebase. Your customers experience them directly on their VR browser, without having to download anything. This also makes it much more practical to maintain and update your code, it is as simple as pushing changes to your webserver, and everyone instantly receives your updates.

## Why develop immersive VR experiences?

You may wonder whether it makes any sense for your content to be presented in an immersive VR experience. When thinking about VR, a lot of people instantly think of games. But there are a lot more experiences that could benefit from full immersion. Think for instance about sports, movies and music. Sure, you can view a match, movie or concert on your TV or smartphone. But as an immersive VR experience, people could actually view this content on a screen the size of an IMAX cinema screen, and you'd still have additional screen real estate that could enrich the experience even further. Let's take a sports match. Imagine not only viewing the match on a massive screen, but when looking to the left having a panel that shows live up-to-date and in-depth statistics and player information while you watch. When looking to the right you could have something like a live twitter feed that shows you what other fans are thinking of the game as it happens. Sure, you can do this on a traditional '2D' website, but this is not a great experience, as this would be at the expense of the size of the video, which people would love to see on a big screen. This repository is a sample project that shows the potential of WebXR for non-gaming experiences.

## What it does

This repository is basically just a website. It's a website that shows viewers the last match West Ham played at their Boleyn Ground stadium in 2016, where they beat Manchester United. As the match is playing, if the viewer turns their head to the left, they can see up-to-date stats about the game. They can even just grab the stats panel and move it anywhere they'd like for optimal viewing comfort. For this example I just kept it simple, by updating the number of goals and yellow cards as they happen. I also generate some fake 'possession' statistics. But the code shows how this could easily be expanded to add much more detailed statistics that could be fed from a live server as the match happens. If the viewer turns their head to the right, they have the option to 'jump-in' to immersive fan experiences of the two teams. These are just simple videos shot with a 360 camera that create a unique fan experience that cannot be replicated on a TV, computer or smartphone. And to add a little more fun to the experience, the viewer can freely float around in the space, and throw a couple of footballs around. You can try the experience yourself by visiting [https://dev.rdcmedia.net/webxr/football/](https://dev.rdcmedia.net/webxr/football/) on your Oculus Quest browser.

## How it works

It uses WebXR and the A-Frame library to create an immersive environment by writing very simple HTML and JavaScript snippets. I've used code written by [Lee Stemkoski](https://github.com/stemkoski), who in turn was inspired by Zach Capalbo for the [VARTISTE](https://vartiste.xyz/) project. I also took some inspiration and borrowed assets created by [Frame Academy](https://learn.framevr.io). Lastly I 'borrowed' some sports videos by West Ham United FC, VReam Sports and Whitecoat Productions. Please do not use this footage for anything, as these videos are copyrighted by their respective owners and I've only used them to demonstrate how WebXR could work, there is no intention to have this site be used by actual viewers or sports fans.

As you will see, the project is a simple index.html that loads up the following libraries:

    <!--Core AFrame Libary-->
    <script src="https://aframe.io/releases/1.3.0/aframe.min.js"></script>
    <!--AFrame Environment to get us a basic environment with lighting-->
    <script src="js/aframe-environment-component.js"></script>
    <!--Handle Oculus Quest Controllers (by stemkoski)-->
    <script src="js/controller-listener.js"></script>
    <!--Handle viewer movement Quest Controllers  (by stemkoski)-->
    <script src="js/player-move.js"></script>
    <!--Handle grabbing and clicking on objects (by stemkoski, slightly amended so objects are clickable as well as draggable, and to make the 'glow' around a focused element work better)-->
    <script src="js/raycaster-extras.js"></script>
    <!--Handle physics (by stemkoski) -->
    <script src="js/physx.release.js"></script>
    <script src="js/physics.js"></script>

With these libraries, your page is instantly set up to be controllable by Oculus Quest controllers, objects you create can have behavior akin to real-life physics, and you instantly have a basic world with a Sun and lighting ready to go. Next, I created a 'room', in which the viewer can watch the match and interact with the other components. To create a room, you only need very basic HTML:

    <!-- Create a floor -->
    <a-box physx-body="type: static;" 
            class="environmentGround room"
            position="0 0.02 0"
            width="20" depth="20" height="0.1"
            material="src: #floor; repeat: 4 4;"
            shadow="receive: true;">
    </a-box>

    <!-- Create a ceiling -->
    <a-box physx-body="type: static;"  
            class="room"
            position="0 10 0" 
            width="20" 
            depth="20" 
            height="0.1" 
            src="#ceiling" >
    </a-box>

    <!-- Create the front facing wall -->
    <a-box physx-body="type: static;" 
            class="room"
            position="0 5 -10"
            width="20" depth="0.1" height="10"
            material="src: #walls; repeat: 2 2;">
    </a-box>

    <!-- Create a back wall -->
    <a-box physx-body="type: static;" 
            class="room"
            position="0 5 10"
            width="20" depth="0.1" height="10"
            material="src: #walls; repeat: 2 2;">
    </a-box>

    <!-- Create a right wall -->
    <a-box physx-body="type: static;" 
            class="room"
            position="10 5 0"
            width="0.1" depth="20" height="10"
            material="src: #walls; repeat: 2 2;">
    </a-box>

    <!-- Create a left wall -->
    <a-box physx-body="type: static;" 
            class="room"
            position="-10 5 0"
            width="0.1" depth="20" height="10"
            material="src: #walls; repeat: 2 2;">
    </a-box>
    
This is basic HTML notation using A-Frame components and positioning. To learn more about how A-Frame components, positioning and rotation work, I recommend you view [Danilo Pasquariello's](https://www.youtube.com/playlist?list=PL8MkBHej75fJD-HveDzm4xKrciC5VfYuV) videos on YouTube. Now that we have a room, we want to place the viewer inside the room and limit the viewer's movement to inside the walls. To limit the movement, I've slightly adapted Stemkoski's movement library so it respects any bounds you provide. These bounds are just basic A-Frame positioning meters. So for the room I created above, I simply provided the following bounds at the top of the HTML to limit the viewer to within the confines of it:

    window.boundaries = {};
    window.boundaries.left = -9;
    window.boundaries.right = 9;
    window.boundaries.up = 5;
    window.boundaries.down = 0.0001;
    window.boundaries.forward = 9;
    window.boundaries.backward = -9;

Then to create the viewer within the room and to make the Oculus Quest controllers usable, I added the following code:

    <a-entity 
        id="player" 
        position="0 2 -0.5" 
        player-move="controllerListenerId: #controller-data;
                     navigationMeshClass: environmentGround;">
    
        <a-camera></a-camera>
    
        <a-entity 
            id="controller-data" 
            controller-listener="leftControllerId:  #left-controller; 
                                 rightControllerId: #right-controller;">
        </a-entity>

        <a-entity 
            id="left-controller"
            oculus-touch-controls="hand: left">
        </a-entity>

        <a-entity
            id="right-controller"
            oculus-touch-controls="hand: right"
            raycaster="objects: .raycaster-target, .environmentGround;"
            raycaster-extras="controllerListenerId: #controller-data; 
                              beamImageSrc: #gradient; 
                              beamLength: 1.5;">
        </a-entity>
    </a-entity>
    
And this is all you need for the viewer to freely move around in the space we've just created! Now, let's create the main screen that viewers can view the match on:

    <!-- Create the video screen on the front facing wall -->
    <a-video id="video-screen"
                class="room"
                material="shader: flat; src: #match-stream"
                geometry="primitive: plane; width: 16; height: 9;"
                position="0 5 -9.9"
                rotation="0 0 0"
                video-screen
                raycaster-target="canClick: true;"
                onClick="playPauseVideo();"
                visible="true">
        <!--Indicate to the user that they need to click the screen to play the stream-->
        <a-image id="play-button" 
                class="room"
                src="#play"
                position="0 0 .1" 
                width="0.75" 
                height="0.75" >
        </a-image>
    </a-video>

I've given this screen the attribute 'video-screen', which means I can now initialize it with A-Frame and add some JavaScript logic to it. I've also added `raycaster-target` as an attribute. This tells the libraries that we've added to treat this object as something that can be interacted with. I simply tell the libraries that it should be clickable, and when the user clicks it it should run a function called `playPauseVideo()`. I've also added a nice play icon to indicate that the user needs to click to start the video. Now, let's fill in the code to handle the user click:

    function playPauseVideo(){
    // Plays or pauses the main video screen in the room
        
        const playButton = document.querySelector("#play-button");
        
        if (!window.streamPlaying && !window.immersivePlaying) {
            window.video.play();
            window.video.muted = false;
            window.streamPlaying = true;
            playButton.setAttribute('visible', false);
    
        } else if (!window.immersivePlaying) {
            window.video.pause();
            window.streamPlaying = false;
            playButton.setAttribute('visible', true);
        }
    }

I simply hide the play icon when the video starts, and I store a global variable that tells any component that a video is currently playing. Notice that this function assumes that the video is a global JS HTML5 video variable that can be interacted with. In order to get the video available as a global variable, I simply ask A-Frame to make it available as soon as it initializes the `video-screen`. While I'm at it, I can even use this A-Frame function to allow the viewer to rewind or fast-forward the video when it is playing with the Oculus Quest Controller:

    // Make main video screen available globally
    AFRAME.registerComponent('video-screen', {
        init: function () {
        const videoEl = this.el.getAttribute('material').src;
        window.video = videoEl;
        this.controllerData = document.querySelector("#controller-data").components["controller-listener"];

        },
        tick: function() {
        if (window.streamPlaying && Math.abs(this.controllerData.rightAxisX) > 0.90) {
            if ( this.controllerData.rightAxisX > 0.90 )
                window.video.currentTime = window.video.currentTime + 15
            if ( this.controllerData.rightAxisX < -0.90 ) {
                if (window.video.currentTime > 15) {
                    window.video.currentTime = window.video.currentTime - 15
                } else {
                    window.video.currentTime = 0
                }
            }

            }
        }
    
    });

And that's it! We now have a fully functional video screen that can be played, paused, rewinded or fast-forwarded with Oculus Controllers! Now let's get to the immersive elements of the page. Let's create a panel on the right wall that can play some immersive fan videos to warm the viewer up before the match starts! To do that, I simply create a panel and add two logos to it. I then make these logos clickable and give them a function to run on click, just as we did with the video screen before:

    <a-box
        class="room"
        width="0.1" depth="7.5" height="5"
        position = "9.75 5 -3"
        color = "#BBBBBB">
        <a-plane src="#west-ham" 
                    raycaster-target="canClick: true;"
                    onClick="play360('west-ham-360-player');"
                    width="3" 
                    height="3"                               
                    position="-0.06 0 -2" 
                    rotation="0 -90 0">
        </a-plane>
        <a-plane src="#man-united" 
                raycaster-target="canClick: true;"
                    onClick="play360('man-united-360-player');"
                    width="3" 
                    height="3"                               
                    position="-0.06 0 2" 
                    rotation="0 -90 0">
        </a-plane>
    </a-box>

To play the 360 videos though, we need some specific A-Frame components that can handle playing them. I'll make these invisible and only show them upon clicking the logos. I give them the attribute `immersive-player` so we can interact with them like we did above with `video-screen`.

    <!--Create the immersive 360 video player-->
    <a-entity id="videospheredisplay" immersive-player position="0 4 0">
        <a-videosphere id="west-ham-360-player" 
                        position="0 0 0" 
                        radius="4" 
                        src="#west-ham-360" 
                        videosphereexpand  
                        visible="false">
        </a-videosphere>
        <a-videosphere id="man-united-360-player" 
                        position="0 0 0" 
                        radius="4" 
                        src="#man-united-360" 
                        videosphereexpand  
                        visible="false">
        </a-videosphere>
    </a-entity>

Now for the logic to handle the clicks. Very similar to the code snipped we used above to play the main video, but this time we also want to do some magic with making the 360 playing A-Frame components visible. We also want to prevent the user from playing both the main screen and the 360 videos at the same time, so we need to handle that as well:

    function play360(id){
    // Plays one of the immersive 360 videos
    
        const video = document.querySelector("#" + id).getAttribute('material').src;
        const object = document.querySelector("#" + id)
        
        if (!window.streamPlaying) {
            
            // Hide all room elements
            const roomElements = document.querySelectorAll(".room");
            roomElements.forEach((roomElement) => {
                roomElement.setAttribute("visible", false)
            });
            
            // Make 360 player visible and start playing video
            object.setAttribute("visible", true)
            video.play();
            video.muted = false;
            
            // Make player available globally so it can be stopped by controllers
            window.immersivePlayer = object;
            
            // Wait for the video to load and then inform the 'tick' function of the player to detect controller presses to stop the video
            setTimeout(
                function() {
                    window.immersivePlaying = true;
                }, 1000
            );
    
        } else {
            
            // Pause the main video first before starting to play 360 content
            window.video.pause();
            window.streamPlaying = false;
            play360(id);
        }
    }

Another thing we have to cover is for the user to be able to 'exit' playing a 360 video. We also want to allow them to rewind and fast-forward as we did with the main video. So for that we again use the A-Frame initialization and tick function:

    // Whenever an immersive 360 video is playing, detect controller interaction
    AFRAME.registerComponent('immersive-player', {
        init: function () {
            this.controllerData = document.querySelector("#controller-data").components["controller-listener"];
        },
        tick: function()
        {
            //Stop playing if any button is pressed
            if (window.immersivePlaying && (this.controllerData.buttonY.pressing ||
                this.controllerData.buttonX.pressing ||
                this.controllerData.leftTrigger.pressing ||
                this.controllerData.leftGrip.pressing ||
                this.controllerData.buttonB.pressing ||
                this.controllerData.buttonA.pressing ||
                this.controllerData.rightTrigger.pressing ||
                this.controllerData.rightGrip.pressing)
            ) {
                // Make room elements visible again
                const roomElements = document.querySelectorAll(".room");
                roomElements.forEach((roomElement) => {
                    roomElement.setAttribute("visible", true);
                });
                
                // Stop playing immersive
                window.immersivePlayer.setAttribute("visible", false);
                window.immersivePlayer.getAttribute('material').src.pause();
                window.immersivePlaying = false;
            }
            
            //Fast forward or rewind with right joypad
            if (window.immersivePlaying && Math.abs(this.controllerData.rightAxisX) > 0.90) {
                if ( this.controllerData.rightAxisX > 0.90 )
                    window.immersivePlayer.getAttribute('material').src.currentTime = window.immersivePlayer.getAttribute('material').src.currentTime + 5
                if ( this.controllerData.rightAxisX < -0.90 ) {
                    if (window.immersivePlayer.getAttribute('material').src.currentTime > 5) {
                        window.immersivePlayer.getAttribute('material').src.currentTime = window.immersivePlayer.getAttribute('material').src.currentTime - 5
                    } else {
                        window.immersivePlayer.getAttribute('material').src.currentTime = 0
                    }
                }

            }
        }
        
    });

And there we have it. Our first immersive interactions are done, with very little code required! To spice up the environment even more, and give the viewer something to do while waiting for the match to start, let's throw in a couple of footballs they can play with. This requires very little code:

    <!--Add some footballs to add to the ambiance-->
    <a-sphere physx-body="type: dynamic;" 
                physx-material="restitution: 0.8;"
                raycaster-target="canGrab: true;"
                physics-grab
                class="room"
                position="1 6 -5" 
                radius="0.5" 
                material="src:#ball; color:white;"
                shadow="cast: true;">
    </a-sphere>
    <a-sphere physx-body="type: dynamic;" 
                physx-material="restitution: 0.8;"
                raycaster-target="canGrab: true;"
                physics-grab
                class="room"
                position="2.5 6 -5.1" 
                radius="0.5" 
                material="src:#ball; color:white;">
    </a-sphere>
    <a-sphere physx-body="type: dynamic;" 
                physx-material="restitution: 0.8;"
                raycaster-target="canGrab: true;"
                physics-grab
                class="room"
                position="3 4 -5" 
                radius="0.5" 
                material="src:#ball; color:white;">
    </a-sphere>
    <a-sphere physx-body="type: dynamic;" 
                physx-material="restitution: 0.8;"
                raycaster-target="canGrab: true;"
                physics-grab
                class="room"
                position="3.5 8 -4.8" 
                radius="0.5" 
                material="src:#ball; color:white;">
    </a-sphere>

I've given these footballs the attributes `physx-body="type: dynamic"`, `physx-material` and `physics-grab` to make them behave like balls in the real world, that bounce around the space. This works because the walls we created above had the attribute `physx-body="type: static;"`. We can even make the balls grabbable so the viewer can throw them around. But for that we need 1 additional bit of code when these components are initialized by A-Frame:

    // Handle the grabbing of objects
    AFRAME.registerComponent("physics-grab", {
        init: function () 
        {
            let self = this;
            this.el.addEventListener("raycaster-grabbed", function() {
                self.el.addState("grabbed"); 
            });
    
            this.el.addEventListener("raycaster-released", function() {
                self.el.removeState("grabbed"); 
            });
        }
    });

This just handles some logic that the physics and raycaster libraries we've loaded in the beginning need to make it all work smoothly. But with this minimal bit of code we're all set, and the user can now bounce balls galore around the space. Pretty cool right?

Now for the final bit of our immersive experience, we want the viewer to have a stats board that shows statistics of the match while it plays. In the code I've added stats like ball possession, yellow cards and red cards, but for this explanation I'll just focus on goals:

    <!-- Add Stats Board -->
    <a-box position = "-9.75 5 -3"
            physx-body="type: kinematic;"
            class="room"
            raycaster-target="canGrab: true;"
            width="0.1"
            depth="7.5"
            height="5"
            match-stats
            color = "#191817">
        <a-plane id="goalscaption" 
                    position="0.1 1.15 .01" 
                    rotation="0 90 0" 
                    height=".6" 
                    width="auto" 
                    color="#191817"
                    text="value: Goals; 
                        font: exo2bold; 
                        width: 5; 
                        align: center">
        </a-plane>
        <a-plane id="goals-team-a" 
                    position="0.1 1.15 0.25" 
                    rotation="0 90 0" 
                    height=".6" 
                    width="auto" 
                    color="#191817"
                    text="value: 0; 
                        font: exo2bold; 
                        width: 5; 
                        align: left">
        </a-plane>
        <a-plane id="goals-team-b" 
                    position="0.1 1.15 -0.23" 
                    rotation="0 90 0" 
                    height=".6" 
                    width="auto" 
                    color="#191817"
                    text="value: 0; 
                        font: exo2bold; 
                        width: 5; 
                        align: right">
        </a-plane>
    </a-box>

You'll notice I've made this board `physx-body="type: kinematic;"`. This will allow the viewer to move the board around as we did with the balls, but instead of the board falling down with gravity, it will just keep floating wherever the viewer wants it! Now all that is left is to update our goals stats as they occur. In the real world, you'd probably want a server to update these values as they occur, using techniques such as [server-sent-events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events). But for this example I slightly cheated, given the match already happened and we know exactly when the goals occurred. So I just simply check the time of the video, and update the digits on the board accordingly:

    // Handle the data displayed on the stats board
    AFRAME.registerComponent('match-stats', {
        
        init: function()
        {
            this.lastUpdate = 0
        },
    
        tick: function()
        {
            if (window.streamPlaying){
                
                // Make the score 1-0 as it happens
                if (window.video.currentTime > 812 && !this.firstGoalSet ) {
                    document.querySelector("#goals-team-a").setAttribute( "text", "value", 1);
                    document.querySelector("#goals-team-b").setAttribute( "text", "value", 0);
                    this.firstGoalSet = true;
                }
            }
        },
        
    });

And that's it! With very little code or complexity, we have a fully functional and engaging immersive web experience! Have a look at index.html to see how all these snippets fit together, and then feel free to use this as inspiration for your own immersive projects. I hope this inspires you to start developing for the Metaverse!