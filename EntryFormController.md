# Entry Form Controller

## Introduction

Entering a new mood log is the main user action in Flourish. Therefore, it is no
surprise this view is the most interactive. In this section we will take our static 
entry form layout and add interactions to allow users to log their mood. We will 
also introduce IBAction methods and IBOutlet objects that we will use to link 
elements of our Storyboard to the code in our view controller. 

New Concepts This Chapter 
* IBAction
* IBOutlet 

## Creating EntryFormController.swift

## Mood Dropdown 

Let's start with what we need for our mood picker.  Our end goal is to get our 
picker to look like this *****add image******. Our dropdown will be built by 
placing the six mood option buttons in front of an image background. As always 
let's begin by defining our constants and variables. The first one we will define 
is a constant for our picker dropdown background. 

~~~language-swift
let picker = UIImageView(image: UIImage(named: "picker"))
~~~

UIImageView class is a container that allows us to display a single image or 
animate a series of images. In this case we want to display an image of type 
UIImage named "picker". The named:Name method of UI image looks for an image file
of the same name in the assets folder of the application. If you spell the name
wrong, your app will still build and run, but you'll quickly find out your image
is missing! Watch for those spelling mistakes when referencing image names. 

Now that we have a constant for our background, we need the mood option buttons. 
Let's begin by defining a collection that contains the all of our mood options. 

~~~language-swift
let feelings = [
  ["title" : "the best", "color" : "#8647b7"],
  ["title" : "really good", "color": "#4870b7"],
  ["title" : "okay", "color" : "#45a85a"],
  ["title" : "meh", "color" : "#a8a23f"],
  ["title" : "not so great", "color" : "#c6802e"],
  ["title" : "the worst", "color" : "#b05050"]
]
~~~

Our collection is an array of dictionaries because each member of the
array has a key-value pair. Each array item has an index which we will use for
our button tag, which will represent the value of the option with low numbers 
representing a better mood and high numbers representing worse moods. 
We will then use the dictionary at that index to retrieve the button title and 
color, which we will use to create the button label.

Once we have our constants, it is time to position and style our picker. Type 
following into the viewDidLoad() method: 

~~~language-swift
picker.frame = CGRect(x: 45, y: 160, width: 286, height: 291)
picker.alpha = 0
picker.hidden = true
picker.userInteractionEnabled = true
~~~

The first line sets the frame of property of our UIImageView. As we previously 
learned, UIImageView is a container for our UIImage, the frame property has the
position of the frame and the size. Our frame is going to be a rectangle, so we 
can use the CGRect data structure to represent the dimension and location of our 
frame rectangle. The x and y properties denote the rectangle's origin, while 
width and height denote size. It's worth mentioning now that iOS apps have a 
coordinate system that originates (point 0,0) from the upper left corner of the 
screen. 

Next we set hide our image view by giving the `alpha` property a value of 0 and 
setting the `hidden` property to true. With alpha, 1 is synonymous with 100% 
opaque, while anything between 1 and 0 will show our UIImageView in varying 
percentages of transparency. You might be wondering why we chose to reduce the 
alpha property to 0 ***and*** set the hidden property to true, seeing as how both 
of these properties make the dropdown invisible. The reason is, that an object 
with an alpha value of 0 is still drawn, and still takes up space in our view. 
It also means that, despite not being visible, it can still inhibit user 
interaction with any objects that are stacked behind it. In our case, simply 
setting the dropdown's alpha to 0 would mean it would block users from interacting 
with the textfield behind it. This would likely cause your users to get angry that 
the textfield is "broken." On the other hand, we cannot simply rely on the hidden 
property because it only has a boolean value to toggle appearance, and we would 
like to animate and fade in our dropdown. Thus, we will leverage alpha for the 
fade in affect and animate its value from completely transparent (0) to fully 
opaque (1).

Finally our last line sets the `userInteractionEnabled` property to true, which
means any touch events on our UIImageView will be registered, rather than 
ignored. Most of the elements in our storyboard have the userInteractionEnabled
property set to true by default. However, when we initialize a new UIImageView
object, the userInteractionEnabled property is set to false by default. We actually
could have drawn the picker in our Main.storyboard file using interface builder and 
set our properties attributes inspector. The reason we did not draw our dropdown
in the interface builder is because we are going to be programatically creating 
the button subviews.

Subviews and superviews refer to a parent-child relationship between views. The
superview is the container for the subview. This means that any coordinates a 
subview has, start from the upper left corner of the superview. It also means 
that any transparency effects on the superview affect the subview. Furthermore, 
it can mean the superview can clip subview contents that would otherwise overflow
the suprview's frame. In this view controller, our picker is going to be 
a subview of the view controller's view property and the mood buttons are going 
to be subviews of the picker. To see how this plays out, we first need to 
actually add the picker subview to the view controller's view property. 

Right below our previous code, let's add: 

~~~language-swift
view.addSubview(picker)
~~~

This adds a subview, picker, to our view controller's view property. All view
controllers have a view property, of type UIView, that can be referenced simply
as view. 

Now that we have created a new subview, let's make sure nothing has gone awry. 
Go back and change the picker's hidden property to true and alpha to 1 and build
and run to make sure our subview is showing up. You should see this:

**** add image ****

Ok we have our picker. Reset alpha to 0 and the hidden to true and let's move on. 
Now that we have the picker, we need to mood buttons that go inside the picker. 
For that we are going to add a subview to picker for each mood option. In the
viewDidLoad method, after your picker.userInteractionEnabled = true statement but
before view.addSubview(picker) add the following: 

~~~language-swift
var offset = 21

for (index, feeling) in enumerate(AppHelper.properties.moods)
  {
    let button = UIButton()
    button.frame = CGRect(x: 13, y: offset, width: 260, height: 43)
    button.setTitleColor(UIColor(rgba: feeling["color"]!), forState: .Normal)
    button.setTitle(feeling["title"]!, forState: .Normal)
    button.tag = index
    button.addTarget(self, action: "setMood:", forControlEvents: .TouchUpInside)
    
    picker.addSubview(button)
    
    offset += 44
  }

~~~

In order to understand what is going on here, we need to revisit our goals. We 
want to stack mood buttons inside our picker image background so it looks like a 
dropdown menu. We can achieve this by spacing the buttons an equal vertical 
distance from the previous one using an offset variable. The offset variable
will represent the y coordinate of the button's frame property. Our initial offset
variable will start at 21, which is allows the first button we are going to create
to clear the dropdown image's tail. 

Now we are going to look at our for-in loop. Using Swift's native enumerate 
function returns a Tuple for each value in the array, giving us an index
variable we can use in our loop. Index refers to the iteration of the loop. Swift's
enumerate function index starts at 0 and increases by 1 each time. We use the 
index variable to set the tag property for each button. A button's tag is simply
and identifier we can use later on to answer the question: "Which button was 
pressed?" 

Now our for-in loop should be making sense. The first thing we do is declare a 
constant equal to a new instance of UIButton. Then we set the frame values for 
the position and the height and width of our new button. Next we set the font
color for our button's label as equal to the color property of the moods array
member we are currently iterating. After that we take the title property of that 
array member and set it as our UIButton's title text. *** DO WE NEEED FOR STATE? ****
Our next line gives the button a unique identifier via the tag property, while
the following line assigns an action "setMood" for the TouchUpInside event. 
TouchUpInside is fired when a user taps a button. Our penultimate line in our 
loop adds our button as a subview on the picker view. Finally, we increment our
offset variable before our next iteration of the loop. 

Before we can build and run to see our beautiful dropdown menu, we need methods 
that will show and hide our dropdown. Here's is our open picker method: 

~~~language-swift
func openPicker()
  {
    self.picker.hidden = false
    
    UIView.animateWithDuration(
      0.3,
      animations: {
        self.picker.frame = CGRect(x: 45, y: 180, width: 286, height: 291)
        self.picker.alpha = 1
      },
      completion: { finished in
      }
    )
  }

~~~

If you recall way back when we wrote the code for our picker, we set it to hidden
with an alpha value of 0. Naturally if we want to show it, we need to set the
hidden property to false and give the picker's alpha property a value of 1. 
The first line of our openPicker method simply sets our picker's hidden property 
to false. The next line could simply be:

~~~language-swift
  self.picker.alpha = 1;
~~~

But that's lame! Why do that when we can easily create a cool fade-in effect? 
Thought so. UIView has a great method called animateWithDuration that takes as 
arguments a duration, an object of properties you want to animate, and a 
completion block. For this we are using 0.3 seconds as the duration of our 
animation and we'll be animating the alpha property as well as the frame property
of our picker. When we instantiated our picker, we set the frame rectangle's y
coordinate equal to 160. We can animate the picker's vertical position simply
by setting a different y coordinate for the picker in the animations object. 
Instantiating at y: 160 and ending at y: 180 is going to make the picker slide down
over the course of the animation. That, combined with our fade-in effect achieved
by going from alpha = 0 to alpha = 1 will make your users smile. 

You now might be wondering about completion blocks. The completion argument 
of animateWithDuration simply denotes a block of code to run after the animation
has completed. In this case we aren't doing anything after the animation so we 
leave the default "finished in" line. 

Now that we have an open picker method, we need a method with the converse of all
of our openPicker commands to have the picker fade out and slide up. Here's the
closePicker() method: 

~~~language-swift
  func closePicker()
    {
      UIView.animateWithDuration(
        0.3,
        animations: {
          self.picker.frame = CGRect(x: 45, y: 160, width: 286, height: 291)
          self.picker.alpha = 0
        },
        completion: { finished in
          if (finished) {
            self.picker.hidden = true
          }
        }
      )
    }
~~~

Ok let's take stock: we have created a picker object and given it buttons as 
subviews. We've added the picker (and by extension it's subviews) to our view 
and we wrote methods for opening and closing our picker. Now we need to call
the open and close methods to show and hide our picker when our app is running. 
To do this we will write a method that calls closePicker() if the picker is open
and openPicker() if the picker is closed. Then we'll have a button in our interface
call that method when a user taps the button. Meet our togglePicker method: 

~~~language-swift
  @IBAction func togglePicker(sender: AnyObject)
    {
      picker.hidden ? openPicker() : closePicker()
    }
~~~ 

Our togglePicker() method uses a handy operator called the ternary operator (?). 
The ternary operator defines a conditional expression. Think of it as short hand
for and if-else statement.  The condition is on the left of the ternary operator, 
a true condition and false condition to the right of the operator separated by a
colon. Using our togglePicker function as an example, we read the ternary statement
as "if picker.hidden is true, call the openPicker method. If false, call the 
closePicker method." 

## IBActions 

You may have noticed that our togglePicker() method has an @IBAction deceleration
before our familiar func declaration. The @IBAction declaration is known as a 
"type qualifier" and it denotes our method is a special type: an IBAction. 
IBAction stands for Interface Builder Action and, as the name might suggest, is 
a method that gets triggered by an object in our interface builder. The object 
that calls the IBAction is known as the sender, which means we can query the 
sender object in our method. "AnyObject" is the return value for the sender. 
Read our function declaration line as "Our function is an IBAction and it is 
called togglePicker and it returns the sender object that called the function."

Now we just have one more step before we take a break to build our project. We
need to connect our select button in our storyboard to the togglePicker method. 
To connect an object to an IBAction, the easiest way is to open up our main.storyboard
file next to our EntryViewController.swift file and dragging an outlet from 
the object to the IBAction. To get a side-by-side of two files, simply click on
the Assistant Editor and open the two files we know. Now in our main.storyboard
file, ctrl-click on the select button to reveal a menu of sent events. Right 
click on the touchUpInside event and drag an outlet to the togglePicker method. 
We've now set the togglePicker method to be called whenever our select button's
touchUpInside event fires, i.e. when a user taps the button. 

Now finally, build and run. Tapping on the select button will reveal toggle our
picker with the mood buttons we created! Now we need to write some more code to 
allow us to set a mood when we tap a mood button. To reflect a set mood, we are
going to change the label of our select button in our interface builder to show
the mood we've chosen in our picker. 

## IBOutlets

In order to change the label property of our select button, we need to reference
the object in our view controller code. In other words, we need to link an 
interface builder object to a variable in a view controller. We used IBActions to
link interface builder events to view controller methods, so it should come as 
no surprise we'll use something similar here: IBOutlets. IBOutlets are declared
with a similar syntax to IBActions. Let's declare an IBOutlet variable called 
feelingButton:

# Entry Form Controller

## Introduction

Entering a new mood log is the main user action in Flourish. Therefore, it is no
surprise this view is the most interactive. In this section we will take our static 
entry form layout and add interactions to allow users to log their mood. We will 
also introduce IBAction methods and IBOutlet objects that we will use to link 
elements of our Storyboard to the code in our view controller. 

New Concepts This Chapter 
* IBAction
* IBOutlet 

## Creating EntryFormController.swift

## Mood Dropdown 

Let's start with what we need for our mood picker.  Our end goal is to get our 
picker to look like this *****add image******. Our dropdown will be built by 
placing the six mood option buttons in front of an image background. As always 
let's begin by defining our constants and variables. The first one we will define 
is a constant for our picker dropdown background. 

~~~language-swift
let picker = UIImageView(image: UIImage(named: "picker"))
~~~

UIImageView class is a container that allows us to display a single image or 
animate a series of images. In this case we want to display an image of type 
UIImage named "picker". The named:Name method of UI image looks for an image file
of the same name in the assets folder of the application. If you spell the name
wrong, your app will still build and run, but you'll quickly find out your image
is missing! Watch for those spelling mistakes when referencing image names. 

Now that we have a constant for our background, we need the mood option buttons. 
Let's begin by defining a collection that contains the all of our mood options. 

~~~language-swift
let feelings = [
  ["title" : "the best", "color" : "#8647b7"],
  ["title" : "really good", "color": "#4870b7"],
  ["title" : "okay", "color" : "#45a85a"],
  ["title" : "meh", "color" : "#a8a23f"],
  ["title" : "not so great", "color" : "#c6802e"],
  ["title" : "the worst", "color" : "#b05050"]
]
~~~

Our collection is an array of dictionaries because each member of the
array has a key-value pair. Each array item has an index which we will use for
our button tag, which will represent the value of the option with low numbers 
representing a better mood and high numbers representing worse moods. 
We will then use the dictionary at that index to retrieve the button title and 
color, which we will use to create the button label.

Once we have our constants, it is time to position and style our picker. Type 
following into the viewDidLoad() method: 

~~~language-swift
picker.frame = CGRect(x: 45, y: 160, width: 286, height: 291)
picker.alpha = 0
picker.hidden = true
picker.userInteractionEnabled = true
~~~

The first line sets the frame of property of our UIImageView. As we previously 
learned, UIImageView is a container for our UIImage, the frame property has the
position of the frame and the size. Our frame is going to be a rectangle, so we 
can use the CGRect data structure to represent the dimension and location of our 
frame rectangle. The x and y properties denote the rectangle's origin, while 
width and height denote size. It's worth mentioning now that iOS apps have a 
coordinate system that originates (point 0,0) from the upper left corner of the 
screen. 

Next we set hide our image view by giving the `alpha` property a value of 0 and 
setting the `hidden` property to true. With alpha, 1 is synonymous with 100% 
opaque, while anything between 1 and 0 will show our UIImageView in varying 
percentages of transparency. You might be wondering why we chose to reduce the 
alpha property to 0 ***and*** set the hidden property to true, seeing as how both 
of these properties make the dropdown invisible. The reason is, that an object 
with an alpha value of 0 is still drawn, and still takes up space in our view. 
It also means that, despite not being visible, it can still inhibit user 
interaction with any objects that are stacked behind it. In our case, simply 
setting the dropdown's alpha to 0 would mean it would block users from interacting 
with the textfield behind it. This would likely cause your users to get angry that 
the textfield is "broken." On the other hand, we cannot simply rely on the hidden 
property because it only has a boolean value to toggle appearance, and we would 
like to animate and fade in our dropdown. Thus, we will leverage alpha for the 
fade in affect and animate its value from completely transparent (0) to fully 
opaque (1).

Finally our last line sets the `userInteractionEnabled` property to true, which
means any touch events on our UIImageView will be registered, rather than 
ignored. Most of the elements in our storyboard have the userInteractionEnabled
property set to true by default. However, when we initialize a new UIImageView
object, the userInteractionEnabled property is set to false by default. We actually
could have drawn the picker in our Main.storyboard file using interface builder and 
set our properties attributes inspector. The reason we did not draw our dropdown
in the interface builder is because we are going to be programatically creating 
the button subviews.

Subviews and superviews refer to a parent-child relationship between views. The
superview is the container for the subview. This means that any coordinates a 
subview has, start from the upper left corner of the superview. It also means 
that any transparency effects on the superview affect the subview. Furthermore, 
it can mean the superview can clip subview contents that would otherwise overflow
the suprview's frame. In this view controller, our picker is going to be 
a subview of the view controller's view property and the mood buttons are going 
to be subviews of the picker. To see how this plays out, we first need to 
actually add the picker subview to the view controller's view property. 

Right below our previous code, let's add: 

~~~language-swift
view.addSubview(picker)
~~~

This adds a subview, picker, to our view controller's view property. All view
controllers have a view property, of type UIView, that can be referenced simply
as view. 

Now that we have created a new subview, let's make sure nothing has gone awry. 
Go back and change the picker's hidden property to true and alpha to 1 and build
and run to make sure our subview is showing up. You should see this:

**** add image ****

Ok we have our picker. Reset alpha to 0 and the hidden to true and let's move on. 
Now that we have the picker, we need to mood buttons that go inside the picker. 
For that we are going to add a subview to picker for each mood option. In the
viewDidLoad method, after your picker.userInteractionEnabled = true statement but
before view.addSubview(picker) add the following: 

~~~language-swift
var offset = 21

for (index, feeling) in enumerate(AppHelper.properties.moods)
  {
    let button = UIButton()
    button.frame = CGRect(x: 13, y: offset, width: 260, height: 43)
    button.setTitleColor(UIColor(rgba: feeling["color"]!), forState: .Normal)
    button.setTitle(feeling["title"]!, forState: .Normal)
    button.tag = index
    button.addTarget(self, action: "setMood:", forControlEvents: .TouchUpInside)
    
    picker.addSubview(button)
    
    offset += 44
  }

~~~

In order to understand what is going on here, we need to revisit our goals. We 
want to stack mood buttons inside our picker image background so it looks like a 
dropdown menu. We can achieve this by spacing the buttons an equal vertical 
distance from the previous one using an offset variable. The offset variable
will represent the y coordinate of the button's frame property. Our initial offset
variable will start at 21, which is allows the first button we are going to create
to clear the dropdown image's tail. 

Now we are going to look at our for-in loop. Using Swift's native enumerate 
function returns a Tuple for each value in the array, giving us an index
variable we can use in our loop. Index refers to the iteration of the loop. Swift's
enumerate function index starts at 0 and increases by 1 each time. We use the 
index variable to set the tag property for each button. A button's tag is simply
and identifier we can use later on to answer the question: "Which button was 
pressed?" 

Now our for-in loop should be making sense. The first thing we do is declare a 
constant equal to a new instance of UIButton. Then we set the frame values for 
the position and the height and width of our new button. Next we set the font
color for our button's label as equal to the color property of the moods array
member we are currently iterating. After that we take the title property of that 
array member and set it as our UIButton's title text. *** DO WE NEEED FOR STATE? ****
Our next line gives the button a unique identifier via the tag property, while
the following line assigns an action "setMood" for the TouchUpInside event. 
TouchUpInside is fired when a user taps a button. Our penultimate line in our 
loop adds our button as a subview on the picker view. Finally, we increment our
offset variable before our next iteration of the loop. 

Before we can build and run to see our beautiful dropdown menu, we need methods 
that will show and hide our dropdown. Here's is our open picker method: 

~~~language-swift
func openPicker()
  {
    self.picker.hidden = false
    
    UIView.animateWithDuration(
      0.3,
      animations: {
        self.picker.frame = CGRect(x: 45, y: 180, width: 286, height: 291)
        self.picker.alpha = 1
      },
      completion: { finished in
      }
    )
  }

~~~

If you recall way back when we wrote the code for our picker, we set it to hidden
with an alpha value of 0. Naturally if we want to show it, we need to set the
hidden property to false and give the picker's alpha property a value of 1. 
The first line of our openPicker method simply sets our picker's hidden property 
to false. The next line could simply be:

~~~language-swift
  self.picker.alpha = 1;
~~~

But that's lame! Why do that when we can easily create a cool fade-in effect? 
Thought so. UIView has a great method called animateWithDuration that takes as 
arguments a duration, an object of properties you want to animate, and a 
completion block. For this we are using 0.3 seconds as the duration of our 
animation and we'll be animating the alpha property as well as the frame property
of our picker. When we instantiated our picker, we set the frame rectangle's y
coordinate equal to 160. We can animate the picker's vertical position simply
by setting a different y coordinate for the picker in the animations object. 
Instantiating at y: 160 and ending at y: 180 is going to make the picker slide down
over the course of the animation. That, combined with our fade-in effect achieved
by going from alpha = 0 to alpha = 1 will make your users smile. 

You now might be wondering about completion blocks. The completion argument 
of animateWithDuration simply denotes a block of code to run after the animation
has completed. In this case we aren't doing anything after the animation so we 
leave the default "finished in" line. 

Now that we have an open picker method, we need a method with the converse of all
of our openPicker commands to have the picker fade out and slide up. Here's the
closePicker() method: 

~~~language-swift
  func closePicker()
    {
      UIView.animateWithDuration(
        0.3,
        animations: {
          self.picker.frame = CGRect(x: 45, y: 160, width: 286, height: 291)
          self.picker.alpha = 0
        },
        completion: { finished in
          if (finished) {
            self.picker.hidden = true
          }
        }
      )
    }
~~~

Ok let's take stock: we have created a picker object and given it buttons as 
subviews. We've added the picker (and by extension it's subviews) to our view 
and we wrote methods for opening and closing our picker. Now we need to call
the open and close methods to show and hide our picker when our app is running. 
To do this we will write a method that calls closePicker() if the picker is open
and openPicker() if the picker is closed. Then we'll have a button in our interface
call that method when a user taps the button. Meet our togglePicker method: 

~~~language-swift
  @IBAction func togglePicker(sender: AnyObject)
    {
      picker.hidden ? openPicker() : closePicker()
    }
~~~ 

Our togglePicker() method uses a handy operator called the ternary operator (?). 
The ternary operator defines a conditional expression. Think of it as short hand
for and if-else statement.  The condition is on the left of the ternary operator, 
a true condition and false condition to the right of the operator separated by a
colon. Using our togglePicker function as an example, we read the ternary statement
as "if picker.hidden is true, call the openPicker method. If false, call the 
closePicker method." 

## IBActions 

You may have noticed that our togglePicker() method has an @IBAction deceleration
before our familiar func declaration. The @IBAction declaration is known as a 
"type qualifier" and it denotes our method is a special type: an IBAction. 
IBAction stands for Interface Builder Action and, as the name might suggest, is 
a method that gets triggered by an object in our interface builder. The object 
that calls the IBAction is known as the sender, which means we can query the 
sender object in our method. "AnyObject" is the return value for the sender. 
Read our function declaration line as "Our function is an IBAction and it is 
called togglePicker and it returns the sender object that called the function."

Now we just have one more step before we take a break to build our project. We
need to connect our select button in our storyboard to the togglePicker method. 
To connect an object to an IBAction, the easiest way is to open up our main.storyboard
file next to our EntryViewController.swift file and dragging an outlet from 
the object to the IBAction. To get a side-by-side of two files, simply click on
the Assistant Editor and open the two files we know. Now in our main.storyboard
file, right click on the select button to reveal a menu of sent events. Right 
click on the touchUpInside event and drag an outlet to the togglePicker method. 
We've now set the togglePicker method to be called whenever our select button's
touchUpInside event fires, i.e. when a user taps the button. 

Now finally, build and run. Tapping on the select button will reveal toggle our
picker with the mood buttons we created! Now we need to write some more code to 
allow us to set a mood when we tap a mood button. To reflect a set mood, we are
going to change the label of our select button in our interface builder to show
the mood we've chosen in our picker. 

##IBOutlet 

~~~language-swift
  @IBOutlet weak var feelingButton: UIButton!
~~~ 

A few things to note with IBOutlet declaration, starting with the weak designation. 
IBOutlets can be strongly or weakly referenced. For this tutorial, all you need
to know is that subviews should be weakly referenced and top level views should
be strongly referenced. Since our button is a subview of the top level view, we
use weak for our deceleration. After that we simply declare the variable as normal,
but you need to be aware that IBOutlet variables are all optionals. That means 
when we declare them, we should implicitly unwrap the variable's value by placing
the exclamation mark after the variable type. Implicit unwrapping, in case you 
forgot, tells Xcode "assume this always has a value, even though it is an optional."
This means from now on we can simply reference the variable name feelingButton 
and its value will be unwrapped each time we use it. Otherwise we'd have to 
explicitly unwrap the optional's value each time we use it in our code. 

Now that we've declared our IBOutlet variable, we need to link it to our interface
builder object to our IBOulet variable. This works very similar to linking IBActions. 
With both our main.storyboard file and our EntryFormController.swift files open, 
Ctrl-click on the select button in the storyboard and drag it onto the variable 
declaration. 

Now we have access to our button in our controller, which means we can change
its label property to our selected mood. Back in our EntryFormController,lets 
write our setMood() method to accomplish this: 

~~~language-swift
  @IBAction func setMood(sender: AnyObject)
  {
    feelingButton.tag = sender.tag
    feelingButton.setTitle(sender.currentTitle, forState: .Normal)
    feelingButton.setTitleColor(sender.titleColorForState(.Normal), forState: .Normal)
    
    closePicker()
  }
~~~ 

In setMood() we are updating the tag, currentTitle, and titleColorForState 
properties of our feeling button to the tag, currentTitle, and 
titleColorForState properties of the sender. The last line in the method calls 
our closePicker() method, which hides our picker. Notice that we have already
linked this IBAction to the touchUpInside event of our mood buttons ***link back
to that line ***

Ok now build and run, you'll notice that when you pick a mood from the picker 
dropdown, our feelingButton's label changes from "select" to the title of the 
mood you've selected!

## Saving Our Entry


## Location


