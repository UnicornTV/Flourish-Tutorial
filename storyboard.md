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

## Adding views to our default project 

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

## Adding images to tab bar items 

We are now going to add our custom tab images to our tab bar. 


