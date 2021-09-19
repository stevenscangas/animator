# Animator by Jakob Philippe and Steven Scangas

Running the Animator:

Main class: Excellence

There are 4 possible program arguments that can be given in the run configuration.
The view and in arguments MUST be selected in order for the program to run correctly.

-view [viewtype]: the type of view to output this animation to
	"visual": opens up a window that has a canvas where all of the shapes are drawn on a 2D plane over time
	"interactive": same as the visual view but with more controls such as pause/unpause, restart, start/stop looping, and change fps. 
	"text": creates an Appendable that shows the characteristics of the animations shapes at each time interval
	"svg": creates an Appendable in the XML SVG style that can be used to run the animation in some browsers 
	NOTE: the text and svg Appendables can be output in the console or a text file can be created to output them


-in [input file name .xyz]: the input file to use for this animation, 
looks for a file with this name in the directory of the animator project

-out[output file name .xyz: the output file to use for this animation, 
creates a file with the name in the project directory
if this out file is not specified, the default out is "system.out"

-speed [fps]: the number of ticks per second to run this animation at
if no value for speed is specified, the default speed is 1 fps. speed cannot be negative

Here are some examples of valid command-line arguments and what they mean:

-in smalldemo.txt -view text -speed 2 : 
use smalldemo.txt for the animation file, 
and create a text view with its output going to System.out , and a speed of 2 ticks per second.

-view svg -out out.svg -in buildings.txt : 
use buildings.txt for the animation file, 
and create an SVG view with its output going to the file out.svg, with a speed of 1 tick per second.

-in smalldemo.txt -view text : 
use smalldemo.txt for the animation file, 
and create a text view with its output going to System.out .

-in smalldemo.txt -speed 50 -view visual : 
use smalldemo.txt for the animation file, 
and create a visual view to show the animation at a speed of 50 ticks per second.

UPDATES:

In the src we included a new main file called VisualSort. This class when run creates a .txt animation of a visual sort algorithm sorting ellipses based on their blue level. This .txt file can then be used as the -in to run the different types of provided animations.

In the resources folder, we also include holiday.txt, our second animation. Happy holidays!

We did not change the model at all from the previous assignment. We changed the way that the view was created as we thought the view created should not have access to the model. We added separate controllers for the interactive view and the rest of the views. We made it so that our timer in the original visual view could take instructions from the interactive view to avoid reusing that code. Although we technically changed the previous visual view implementation, it realistically did not affect its function at all and in no way can be called by a user only using the visual view and not the interactive view. Everything else remained the same. 

HOW THE PROGRAM WORKS (GENERALLY):

The Interface "model" that stores all of the information about the animation is the IAnimatorModel. 
This model uses a builder subclass/method in order to take in information using the AnimationBuilder interface,
then creates an instance of this model with it. The data in the model is organized into two main lists of interface: IShapes and IMotions.

A shape object contains data about the Name, Type, Position, Dimension, and Color of the shape it represents.
A motion object contains data about the starting and ending characteristics of a specific shape from a specific start and end time.

The IAnimatorView is the interface that represents a view for the model. There are 3 main types of views at this point.
Visual: This view draws the image of each shape at each time specified by its motions onto a 2D canvas over time.
This visual view loops through all of the motions, "tweens" them, and then asks them to "render" themselves onto the graphics object.
Textual: This view represents a textual representation of each shape's characteristics at each point in time.
SVG: This vector-based graphical XML format of the view can be used to run the animation on internet browsers.

The Visual and Textual view utilize a class "Tweening"'s method tween() in order to determine what a shape looks like over time,
based on what the values in the motions have specified. It uses a linear model in order to calculate the exact
values of the shape decided by the motions, at each specific time interval the animation occurs. 

When the main method is executed, multiple things happen. First, a model is created by parsing the input file's data.
Then, a view class is created of the specified input type using an AnimatorViewCreator class. This model and view are then
passed into the IAnimatorController implementation, as well as the speed in fps, and the controller executes its
start method which runs the animation in whatever view/out was chosen.

