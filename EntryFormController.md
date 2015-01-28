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
picker to look like this *****add image ******. Our dropdown will be built by 
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
Let's begin by defining an array that contains the all of our mood options. 

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

Our dictionary is actually an array of dictionaries because each member of the
array has a key-value pair. Each array member has a title which we will use for
the button label, and as well as a color that corresponds to the feeling. 

Once we have our constants, it is time to position and style our picker. Type 
following into the viewDidLoad() method: 

~~~language-swift
 	picker.frame = CGRect(x: 45, y: 160, width: 286, height: 291)
    picker.alpha = 0
    picker.clipsToBounds = true
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

Next we set our opacity by giving the alpha property a value of 0. 1 is fully 
opaque, while anything between 1 and 0 will show our UIImageView in varying
degrees of transparency. Setting the property clipsToBounds as true means anything
inside our UIImageView container that is bigger than the container's size will be
cut off. This comes into play if your UIImage inside of your UIImageView could
potentially be larger than the UIImageView's frame. Setting clipsToBounds to false
would allow for overflow. Next we want to set the picker to hidden, as we only
want the dropdown visible when the user touches a button to toggle the dropdown. 

You might be wondering then, why we chose to reduce the alpha property to 0 and 
set the hidden property to true, seeing as how both of these properties make the 
dropdown invisible. The reason is that an object with an alpha value of 0 is still
drawn, and still takes up space in our view. It also means that, despite not
being visible, it can still inhibit user interaction with any objects that are
stacked behind it. In our case, simply setting the dropdown's alpha to 0 would 
mean it would block users from interacting with the textfield behind it. You'd 
have your (hopefully) millions of users angry that the textfield is "broken." On 
the other hand, we can't just rely on the hidden property because we want to 
fade in our dropdown, which requires setting an alpha value of 0 then animating
to a fully opaque value of 1. 

Finally our last line sets the userInteractionEnabled property to true, which
means any touch events on our UIImageView will be registered, rather than 
ignored. All of the elements in our storyboard have the userInteractionEnabled
property set to true by default. However, when we initialize a new UIImageView
object, the userInteractionEnabled property is set to false by default. We actually
could've drawn the picker in our main.storyboard file using interface builder and 
set our properties attributes inspector. The reason we didn't draw our dropdown
in the interface builder is because we are going to be programatically creating 
subviews. 

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

    for (index, feeling) in enumerate(feelings)
    {
      let button = UIButton()
      button.frame = CGRect(x: 13, y: offset, width: 260, height: 43)
      button.setTitleColor(UIColor(rgba: feeling["color"]!), forState: .Normal)
      button.setTitle(feeling["title"]!, forState: .Normal)
      button.addTarget(self, action: "setFeeling:", forControlEvents: .TouchUpInside)
      
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

Now we are going to look at our for in loop. Using Swift's native enumerate 
function allows returns a Tuple for each value in the array, giving us an index
variable 
