Modular Input v 1.0 by Konstantin Yanchev
Special thanks to Michael Nguyen for all the help!

Modular Input is a simple input system that distributes user input in a hierarchical manner.

It consist of 3 main parts:
	- Controllers: the different controllers provide bindings between user defined actions and Inputs. For example binding the validate action to the Return key or the A button of an Xbox controller.
	- Input context: a context links an input received from a controller to a certain user defined action. The contexts can be either low or high priority.
	- Input manager: the manager distributes the user input from the controller to all the contexts. It can also detect when the input method changes (from a joystick to keyboard + mouse for example).
	
The easiest way to understand the system is to look at the InteractableItem.cs script. The 3 main components are its context, its controller and the InputManager. 

The Input Context provides a way to bind user input to actions. This is done via the "onInput" delegate. Use this delegate to provide the desired implementation for user interaction. The implementation is a function with a bool return type that should return true if the input is consumed for this frame, or false if the input should propagate to lower input contexts. In the script the arrow keys will be used to change the position of items, while the movement keys (WASD) will be used to change color. 

The InputManager provides the current active controller, as well as a notification when the current active controller changes. It also handles how user input propagates through all the currently active input contexts. The input will always be given to the most recently added High Priority input context. If no High Priority input contexts exist, the input is passed to the most recently added Low Priority input context. IMPORTANT: if both High and Low priority context are currently present, the input will only be given to the High Priority input contexts. Each input context can choose to consume the input, by returning "true" from its OnInput method or propagate it to the other contexts below it by returning false.

The Controller binds basic input such as key strokes or button pushes to user action. In this example there is a Standalone controller that checks for keyboard input as well as an XboxOne controller that checks for input from an XboxOne joystick. Note that in order for the XboxOne controller to work, axes need to be defined in the project's Input settings.

The short demo scene shows how the system works. Pressing Enter or clicking on the button will display a new sub menu. It contains a square that changes color, a circle that moves as well as the instructions. Both the square and the circle run the InteractableItem.cs script. If you use a debug inspector window to check the _Manager game object, you can see the input contexts that are currently used: Circle Context being on top, followed by Square Context and finally Demo Context. The circle is moved with the arrow keys. Since it doesn't have any action for the movement keys (WASD), this input is propagated to the context below it - Square Context. This in turn changes the color of the square.
Pressing the Enter key shows a popup that contains a High Priority Context. The popup only reacts to the arrow keys and not the WASD keys. But since it has a High Priority Context, input from the movement keys won't be propagated to the Low Input Contexts and the square won't change color.

 


