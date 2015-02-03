## Designing our Interface

### Introduction

In order to kick off our project, we need to design our user interface (UI). By the 
end of this section we'll have all of the views we need for Flourish set up and 
we can spend the rest of the tutorial adding interactivity and functionality. 
Thinking back to our MVC section, we are now creating views. This means we'll have
mostly static UI elements that will later become more interactive and dynamic
when we create controllers for each view. In Xcode, the easiest way to create 
our UI is using storyboards. Storyboards are a visual tool for laying out 
application elements using a drag and drop interface and a series of menus. 

Jump to your main.storyboard file to get acquainted. When we created our project, 
we said we were making a tabbed application, which means xcode has already given 
us a few things in our storyboard. First, notice we have several labeled boxes
with connectors between them. This is called a scene. 

*** image of a scene ***

A scene consists of a view and a view controller. The view is represented visually
and is what we're going to drag elements onto. The view controller will be a
.swift file where we'll later add things programmatically. Before anyone gets 
confused let's clear something up: everything in a storyboard can be done
in code. Our storyboards are just a visual way for us to create swift code. Now
looking at our storyboard sidebar, we have a list of three scenes: First Scene, 
Second Scene, and Tab Bar Controller scene. Generally, each view controller in
our storyboard is going to have a .swift file associated with it for us to add
code pertaining to that storyboard. However, looking at our project navigator, 
you see we have FirstViewController.swift and a SecondViewController.swift file 
and no file to correspond to the Tab Bar controller in our storyboard. We will
eventually add a file for this controller, but there is still a controller in our
project. Elements that are dragged onto a storybard have default functionality
and if we are not extending that functionality, we often don't need to create a
.swift file for that controller. 


Hit the play button in the upper left hand corner your xcode window to build and 
run the project. This will compile our code into an app and install that app onto
the device specified in the build scheme. You can edit the target device by 
selecting the dropdown menu in the active scheme editor. 

*** show image ***

Let's select iPhone 6 as our target device and run our app. This will launch our 
iOS simulator in a separate window and run our app. You'll see  our scenes and 
the tab bar that we can use navigate between them. Now that you're comfortable 
with what xcode gives us as a default, let's go back to xcode and start adding 
views. 

### Adding views to our default project 

Our app is going to consist of 5 views: 
* Authentication
* My Entries
* New Entry 
* Calendar
* Trends
* Settings

All of these views, with the exception of the authentication view, are going to 
be tabs in our tab bar. Currently our tab bar only has two views, so let's add 
three more. 

In our main.storyboard file, you'll want to select the object library in the
lower right corner of your screen. Drag a view controller from the object library
and drop it anywhere in your storyboard. This creates a new scene in your 
storyboard. Like all scenes, our new scene has a view controller and a view. 

*** image with dragging and highlighting object library ****

Ctrl-click on the Tab Controller Scene and drag an outlet onto your new view 
controller scene. When you drop the outlet on your new controller, a menu will appear
asking you to establish a relationship between your Tab Bar Scene and your new
view controller scene. Under "relationship segue", select the "view controller"
option. Once you do that you'll notice your tab bar controller now has a third tab 
option. Let's add a label to this new view by dragging a label object from the
object library and drop it onto our new view controller scene's view. Next, double
click on the label to change the label text to "Calendar view." After that, double
click on the tab bar item's name and change the name from item to calendar. 
Build and run your project to confirm we've added a third view.  

Now we need to add two more views so follow the same procedure as we did for our
calendar scene to create a trend scene and a settings scene. Now that we've added
our tab views, let's rename the labels for the tab bar items currently labeled
first and second to be "entry" and "journal."

*** pic ***

### Adding images to tab bar items 

We are now going to add our custom tab icons to our tab bar. Xcode image assets
are all stored in a images.xcassets folder, which you can find in the project 
navigator. Open up images.xcassets and you'll see the square and circle images
that are used in the default tab bar icons. This isn't an illustration tutorial, 
so you can go ahead and download our 
[icon pack](https://dl.dropboxusercontent.com/u/80807880/Icons.zip). Unzip the
file and you'll have an icons folder. Drag the icons folder into your xcode 
project's images.xcassets. You'll now see your icons in the images.xcassets folder. 

*** add image ***

If you are adding your own icons, please follow the [Icon and Image Sizes 
guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html) 
from Apple. 

Now that we have our image assets in our project, we can go back to main.storyboard
and assign some of those images to our tab bar items. In your entry scene, click
on the tab bar item. Once selected, turn your attention to the attributes 
inspector's "Bar Item" section. There, you'll see an image field with a dropdown
menu. The dropdown options correspond to the names of your image assets in your
image.xcassets folder. If you're using our icons, select the "edit" icon for this
tab bar item. Now repeat this process to select the appropriate image for the
rest of the tab bar items. 

The images for each tab bar item are as follows:
* Entry Scene - edit icon 
* Journal Scene - contacts icon
* Calendar Scene - calendar icon
* Trends Scene - pulse icon 
* Settings Scene - settings icon 

### Auto Layout 

If you build and run our project so far, you'll notice that the two default tabs
have labels neatly centered in the middle of the screen, while the labels we added
to our new tabs appear in a different place than where we dropped them into the
scene. This is because we haven't really positioned our label yet. In order to 
make it easier to design interfaces for multiple screen sizes, the wiz kids at 
Apple gave us Auto Layout. As Apple describes "Auto Layout is a system that lets 
you lay out your app’s user interface by creating a mathematical description of 
the relationships between the elements. You define these relationships in terms 
of constraints either on individual elements, or between sets of elements." 


{x: auto_layout}
Before moving on, familiarize yourself with the ["Auto Layout menus in Xcode"](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/WorkingwithConstraints/WorkingwithConstraints.html#//apple_ref/doc/uid/TP40010853-CH8-SW3) 

Now let's go through a concrete example of auto layout by creating our new entry
form UI. Here's a wire frame for this view:


As you can see we have a text field for entering a title for your entry, a
select button to select a mood from a set of options, a text field for the body
of your journal entry, and a save button to save your journal entry. Let's select
our new entry scene and begin building our interface. 

{x: clean_slate}
First delete the label that is currently in the New Entry view.

{x: background_color}
Select your Entry view and change the background color to a dark blue in the attributes inspector.
If you're wondering, we used RGB(21, 69, 110) for the production app. 

{x: entry_global_constraints}
Drag a text field object from the object library into your New Entry view and 
add the following spacing constraint values in the Pin menu.

![Entry_general_constraints](https://dl.dropboxusercontent.com/u/80807880/tuts_images/general_constraints.png)

Make sure to select "All frames in container" from the update frames dropdown in
the Pin menu. This option sets these constraints for all objects in our view, 
not just our text field. What we've just done is established a relationship our 
text field object and its nearest neighbor. We've set it so any element will be 
15 points below any element above it and 17 points to the right of any object to its 
left. It also is 0 points away from objects to its right, this means that our 
objects will fill any empty space to their right until it hits another object.  

Furthermore, we specify some constraints that just apply to our text field. 

{x: entry_global_constraints}
Drag a text field object from the object library into your New Entry view and 
add the 40 point height constraint values in the Pin menu

![Entry_general_constraints](https://dl.dropboxusercontent.com/u/80807880/tuts_images/text_field_constraints.png)

Make sure you select "items of new constraints" from the
update frames dropdown in the Pin menu to make our change only apply to the
selected object, in this case our text field. You can check if you've done the
last two steps right but looking at your document outline of your storyboard file. 
You should have three general constraints and one constraint for your Round
Style Text Field. 

{x: doc_outline_check}
Compare your document outline to the following image:

![document_outline_check](https://dl.dropboxusercontent.com/u/80807880/tuts_images/doc_outline_check.png)

{x: change_label_text }
In the text field's attributes inspector, change the text property to "Date".

We are now going to build the mood selector field. This involves first creating
a divider between our text field and the mood selector. 

{x: add_first_divider}
Drag an view object from the object library into your New Entry view directly 
beneath your text field give it a height constraint of 1px. In its size 
inspector, give it a width of 568 points.

We now want to establish a relationship between our text field and the divider. 
What we want to do is set constraint that impacts both of these objects specifically
and keeps them 1 point apart vertically. 

{x: space_field_divider}
Select both the text field and the divider and add the a 1px vertical constraint
in the pin menu. Notice all of the constraints have the word "multiple" as a 
placeholder, denoting this applies to more than one object. 


![multiple_constraints](![document_outline_check](https://dl.dropboxusercontent.com/u/80807880/tuts_images/doc_outline_check.png))

Set the constraint for "items of new constraints" and you'll have your divider 
1px beneath the text field and you have one more general constraint added to 
your document outline. This is a good time to note that clicking on a constraint
reveals which objects are impacted by that constraint in the Size Inspector.


Now let's add an icon for this field to let our users know this is a text field. 

{x: add_title_icon}
Drag an image view object from the object library into your New Entry view so that
it is directly to the left of our text field and set a width of 13 points and a 
height of 18 points in the image view's size inspector. 




