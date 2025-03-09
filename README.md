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

The first step for me was to handle the resizing of the frame. My initial idea to solve this was to implement an algorithm using pythagoras
theorem to work out how far back to move the camera along the Z axis to keep everything in frame as desired. However, to do this I would
need to know the edge of the screen on the plane that our team's 2D game was taking place from, which itself necessitated another algorithm.

The key to my success in this came to be fully understanding how the basic camera I was given worked. I needed to calculate the frustrum 
width and height on a specific plane, which itself became a math problem. I had access to the cameras distance backwards along the Z axis
and the FOV of the camera. Using half the FOV and the Z value, we have an angle and a side in a right angle triangle, which is enough
information to calculate the side we are looking for using the equation *half height = z * tan(half-FOV)*.

