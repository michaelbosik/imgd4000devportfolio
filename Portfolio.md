[checktutorial]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/checktutorial.png
[classstructure]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/classstructure.png
[createnotification]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/createnotification.png
[firstbp]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/firstbp.png
[firstbp1]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/firstbp1.png
[notifications]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/notifications.gif
[tutorialstages]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/tutorialstages.png
[widgetcreation]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/widgetcreation.png
[inventory]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/inventory.png
[openinventory]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/openinventory.png
[loaditems]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/loaditems.png
[resume]: https://github.com/michaelbosik/imgd4000devportfolio/raw/master/images/resume.png

# IMGD 4000 - Development Portoflio
### Michael Bosik - Computer Science / IMGD major

## **Project Description**

Cartograbird is an open world exploration game in which the player is tasked to find the scattered parts of a crash landed explorer. The game has flight mechanics for the main character Carmen to soar around the world in search of these parts. The player can look at their map to orient themselves and document their exploration.

## **My Contribution Summary**

My main category of work done on this project was the user interface, or UI. I was dedicated the role of creating all of the buttons, screens, controls and notifications that a player would see on their screen during gameplay (besides the map).

The first step we took as a team to determine our UI, was to come up with a list of possible elements that our game would need.

* Start Menu
* Option Screen
* Control Screen
* Species Log Screen
* Map

Our original game concept involved a mechanic for identifying animal species that would be in the world. This was later taken out of the scope.

Our final list of UI elements is as follows:

* Main Menu
* Pause Menu
* Event Notifications
* Map
* Help Screen
* Inventory
* Tutorial Sequence

## **I guess I'll learn blueprints...**

To begin creating a UI for the game I did some research on how a user interface is typically created in Unreal Engine. I had originally desired to create everything in C++ as I was aching to get my hands on some code but alas, every example, tutorial, forum even the Unreal documentation said a user interface is done by use of blueprints.

Being first introduced to Unreal Engine's blueprints system at first made me think of Scratch and its drag and drop visual programming. I thought of it as being too basic and not at all complex or functionable. I thought my part could get done in a days work with no hiccups or confusion...Boy was I *wrong*.

Unreal Engine's blueprints is a system of nodes that each have different functionality and can be connected to each other through carfully wired links. Basically, a blueprint function works the same as a function in a C++ class, just with a more visually aesthetic look and complex structure.  Getting used to the concepts blueprints offered took a lot of trial and error as well as just coming to understand the many different possible nodes I could be using in my program.

## **Class Structure**

When I first started thinking about how to go about handling UI, I wanted to have a main hub or control area to handle all user input and UI functionality. So, I made an actor called the UIController that essentially could be placed anywhere in the scene and act as this command center for the UI. This UIController could create and call the functions of all the UI widgets and blueprints, thus any external classes could access these widgets through the UIController.

Here is a basic diagram of the class reference structure:

![Class Reference Structure][classstructure]

## **Menus**

The first widget I created in our project was to display the pause menu. A widget has two main components: the visual design, and the blueprint graphs. To design the pause menu it looked something like this:

![Pause Menu][widgetcreation]

As you can see, the screen consists of a border element, containing a verticle box with a title and 3 buttons. I found that the structure of a widget is similar to that of a webpage with elements contained inside of divs which can be styled seperately and contained in bigger divs. I implemented textures for the menus later on which can be seen [lower](#the-inventory) in this document.

Interaction with a widget is programmed directly into the buttons seen in the designer window. Unreal Engine allows you to attach functionality to elements with the click of a button. Once connected, you can design the blueprint of the button however you'd like. Here is how the resume button works in the pause menu:

![Resume Button Function][resume]

Creating a menu widget ended up involving two main functions. One function, called upon startup of the game, was to create an instance of the PauseMenu.uasset widget in the game. The widget was assigned to a variable and then hidden from view. The next function was called whenever I needed to display the pause menu. The OpenPause method sets the game to be in the **PAUSED** state, so that time does not progress ingame, adds the pause menu widget to the users viewport, sets the game input to UI so that other keyboard controls have no effect on game functionality and finally shows the mouse cursor so the user can click on buttons and plays a sound.

![First Blueprint Part 1][firstbp1]
![First Blueprint Part 2][firstbp]

These two functions were the baseline for generating and displaying all other widgets in our game with necessary tweaks and additions depending on what the widget required.

For example, opening the inventory screen requires that all other UI elements become hidden before doing so, such that there isn't overlap. In our game specifically the only potentially open elements would be the map and the help screen. The pause menu and inventory would be impossible to open together since they both disable game controls when they are open.

To go around this conflict, I set each of the widgets to a boolean variable so say if they were opened or not and added that check to the open inventory method. They were to be closed before the inventory was opened.

![Opening Inventory][openinventory]

## **The Inventory**

On the topic of our games inventory, development of this widget went through many stages. At first I had attempted to design a structure for which the inventory would use to store items picked up by the player. These items were to have an ID, description, image, name and amount tied to them in the structure. Eventually these ideas were tossed aside as I realized our game did not require such a complexley designed inventory.

The progress of our game had shown that we would only ever have the player acquire at most three objects (plane parts) at any given point in the game. I figured this was not enough to warrent an advanced structure of potential items with all of their attributes, rather a defined grid of 6 slots (in case we added more parts). The inventory UI ended up looking like this:

![Inventory Design][inventory]

These 6 slots each had an image property that could be changed by the UIController. The way I set it up is that when the player interacts with an item, the item's *InvImage* attribute, which is a texture png, gets added to the UIControllers inventory array of textures. Whenever the user opens their inventory, the inventory array is parsed for each texture and assigned to the slots in the UI. This is done with this lovely blueprint:

![Loading Items into Inventory][loaditems]

## **Notifications and Tutorial**

One of the main feats of my work on this project was the implementation of the game's tutorial stage when the player first starts the game. This idea started when I implemented the feature of notifications, or pop ups with information to display to the user. These notifications were mostly intended as helpful hints or quest advancement announcements as a sort of way for the game to talk to the player. A notification basically takes in a string as an input and assingns it to the text element within the widget designer, as well as a boolean to say whether or not the notification self destructs. I learned how to animate widgets in the designer to create this sort of slide in and out effect:

![Animating a Notification][notifications]

![Creating a notification][createnotification]

If you look at the above blueprint, youll see that before creating a new notification, we check to see if there are more than 0 pre-existing notifications. If there are, we initiate the MoveUp animation before destroying the notification. If the notification does not already self destruct, we destroy it.

The reason why a notification does not always self destruct was for the purposes of the tutorial. Creating a tutorial for the game went through many different stages of development. At first I tried the struct approach again similar to the inventory, by having a tutorial item contain an index, completion condition and string. This again became too complex so I decided to store the stages of the tutorial as strings in an array of strings.

![The structure of the tutorial][tutorialstages]

I wanted the tutorial stages to advance when a certain condition was met such as pressing the correct key that was prompted. This seemed like it would be easy as all I would have to do is create event listeners for the ActionEvent keypresses such as Space, E, I, etc. I stored these conditions in an array of booleans that matched the stages of the tutorial. I could then easily check the condition array to see if we should advance the tutorial like this:

![Advancing the tutorial][checktutorial]

However, when I had these listeners in the UIController, I noticed they no longer performed the actions they were supposed to in game such as jumping, opening the inventory and interacting with actors. This is because no event in the game can be listened to in more than one location. This would be multithreading the event which Unreal Engine does not support.

## **Conclusion**

Creating this game was not only an educational experience, but I was able to fully jump on board with the concept of blueprints, so much so that I now prefer them over text programming. The aesthetic and visual functions that you can create are far more entertaining and easy on the mind to look at all day and they are overall easier to understand. I learned a lot about many aspects of the Unreal Engine and feel like I've gained a much better understanding of properly scoping a game and dividing work amongst team members to accomplish goals in a timely manner.

