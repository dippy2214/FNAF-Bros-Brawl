# FNAF-Bros-Brawl
This is Fnaf bros brawl, a platform fighter for the PS5 (no code, unfortunately sony takes issue with that) by me, Patrick, 
Iris and Logan for the CMP208 module at abertay university.

<a href="https://www.youtube.com/watch?v=_tTr5jb77O8">
  <img src="https://img.youtube.com/vi/_tTr5jb77O8/maxresdefault.jpg" alt="Watch the video" width="400" height="225">
</a>

https://www.youtube.com/watch?v=_tTr5jb77O8

## 🛠 The Development Process
### 🙋‍♂️ Meeting The Team 
At the start of the process, we had an immediate issue - we had a group of four programmers, none of whom had any sort of training
in management roles or working in teams. The most experienced in team coordination in our group was Patrick, who had worked with Iris
before on some game jam projects. Me and Logan joined them for this project, being on friendly terms, and both of us had up to this
point little experience making full games aside from a first year module (which admittedly left something to be desired). 

### 🏃‍♂️ And So We're Off
Quickly we realised that to play to our strengths as a team we would benefit from each taking some core system of the game and 
working on it ourselves, with discussions over the minimal integration where required. Later in development, we would work more 
closely together as we moved to smaller features than the core systems which required more integration and teamwork. 

I was given ownership of the camera system for the game to start with - which had to dynamically resize to fit all players on the 
screen, work for 1-4 player characters, and not show space outside of the edge of the playable area. However, immediately and for
every single member of the group, the first major challenge of this project reared it's head: the skateboard engine.

For development on this project, and to target the PS5 as a platform, abertay had decided to roll out the skateboard engine, an in house
custom made engine to be used as an educational tool. Unfortunately for us, we were the first year group to experience skateboard. At 
the start of our project, it was in a slightly unfinished state, and for various features students had to go into the engine's code and 
fix up a feature that was broken (I myself experienced this when our group wanted to transition scenes, and so I had to go into the
engine's code, understand how they had implemented scene transitions, and fix the bug that had caused it to break for us). I do think
that this taught me a lot about how the systems we were using worked on a lower level, but it did mean that progress in the early weeks
of the project was slow, as the time was spent understanding the basics of the engine through making small and simple things that would 
not make the final build.

Skateboard engine also utilizes an ECS, which at the time none of us had any experience with. Wrapping our heads around this required a
heated (but not aggressive) discussion with the whole group and module leaders, which admittedly was quite enjoyable (although from the 
outside it may have looked like we were falling out, this was absolutely not the case for all involved, and we made sure to have a follow
up conversation afterwards to make sure we were all on the same page). This added to the learning time we all needed, but in the end we 
found the ECS to be quite a useful system, and many of these concepts would be applied to later unity projects we made together.

### 🪀 Getting Into The Swing Of Things
After a while (roughly week 4 of the module) the team felt comfortable enough with the skateboard engine to make our project repo and 
start development. I quickly set up my test scene with some simple moving objects with a 'PlayerDataComponent' component and 'Transform' 
component (as agreed upon with the group, the way the camera would find player characters would be by searching the engine's registry 
for entitys with these components), and started designing the camera system. Of course, skateboard provided a simple camera framework 
which I could use as a starting point, but this camera didn't have any more than the basic requirements (the ortho mode didn't even work
when we were using the engine).

The first step for me was to handle the resizing of the frame. My initial idea to solve this was to capture the player position data by
looping through the entitys with the desired components and storing the largest and smallest x and y values (since it is only the players 
closest to the edge that I care about for the purposes of this algorithm), and then to implement an algorithm using pythagoras theorem 
to work out how far back to move the camera along the Z axis to keep everything in frame as desired. However, to do this I would need to 
know the edge of the screen on the plane that our team's 2D game was taking place from, since skateboard handled a 3D space, which itself 
necessitated another algorithm.

The key to my success in this came to be fully understanding how the basic camera I was given worked. I needed to calculate the frustrum 
width and height on a specific plane, which itself became a math problem. I had access to the cameras distance backwards along the Z axis
and the FOV of the camera. Using half the FOV and the Z value, we have an angle and a side in a right angle triangle, which is enough
information to calculate the side we are looking for using the equation *half height = z * tan(half-FOV)*. 

With this, I could implement the original algorithm I envisioned, through use of interpolations. I made the quick decision to simply set 
the camera to the desired position, but this led to me creating some buffer values to keep the players slightly more on screen (so that 
they didn't hug the edge of the screen whenever the camera was zooming out) and defining limits on how far the camera would be willing to
zoom out and, more importantly for our game's purposes, how far it would zoom in. But with that the feature was done.

### 👟 Two Steps Forwards
Now it was time to start on the second important feature of the camera: moving around the screen to follow the players. It was one thing
to keep all players in frame, but if it didn't move to center around the player characters then things would look and feel very strange 
to players - especially if the two players are on the same side of the stage, as so often happens in a platform fighting game. 

To tackle this, my immediate idea was to average the max and min positions on the x and y axis that we gathered for the previous algorithm.
This would find the center point between the furthest apart players, keeping the camera feeling natural and using data I already had.
After implementing this solution however, I encountered an unexpected issue. My camera screen resizing code was completely broken. After
commenting it out and testing only my code to move the focal point, I found it was working correctly, but there was a deeper issue. My
code for resizing the screen had been written with the expectation that the focal point of the screen would always be (0,0). It was
time to take a step back and do some refactoring.

### 🔧 Fixing Up Old Mistakes
After an embarrassingly long time, I realised I could simply normalise my min and max positions by subtracting the camera's x and y
coordinates. I still had plenty of fun issues though, such as forgetting to add default values and exploding the cameras position on 
startup - an issue I only noticed when fixing a different issue caused by setting the camera's position exactly. Instead, the program 
now lerped towards a target position, creating a much smoother movement.

### 🌍 Meeting The Real World
At this point in development, the team decided it was worth integrating what we currently had into one project which we could all build 
off. We had all made decent progress in our own areas, and Iris was reading to start working on a wider variety of tasks required for 
making the whole game rather than just core systems. As such, I now had real player characters to test with, curtesy of Patrick.

And immediately it caused me some problems.

What started happening was that now that the players could move freely, and were recieving knockback from Logan's attacking system, they 
could move even faster than the camera could resize. A painful oversight, and one that would affect everyone elses ability to debug their
own functionality, so I made sure to come in early the next day and implement a fix. Fortunately, as you may have guessed, this was a
simple problem to solve, but I hope this paragraph can convery the panic I felt when I recieved a message shortly after leaving saying 
that my teammates couldn't see where the players were, with no further context. Thanks, guys 🙂.

### 🛣 Moving On
I have a confession to make to you now, reader. I actually got quite attached to this camera system. I had spent the last few weeks getting
really into the ins and outs of how it worked, and had a lot of fun working on the algorithms above. I wanted to keep working on it.
Fortunately, Patrick was there to bring me back to reality. In the future this would actually be an interaction we had quite a bit, which
caused a few disagreements between us (although admittedly he was often the one who was the most correct), but this was one of our first 
times working together. As I started to drift into obsession, he calmly dragged me back to reality by reminding me that our group was not
in fact making a camera, but a whole game.

After this, I moved away from polishing my already perfectly good camera, and started working with Iris on our menus and basic user 
functionality. This is the part of the process where I fixed the issue in the engine with scene transitions, while Iris was designing a 
button system and the main and pause menus. The issue actually turned out to be a simple problem while reassigning a pointer which 
represented the current scene, although this took me quite a while to debug as I had to understand what was happening within the engines
codebase.


