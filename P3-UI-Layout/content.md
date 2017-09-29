---
title: "UI Layout"
slug: ui-layout
---

We'll start building our app by implementing the UI in _Interface Builder_. For reference, here are the tip calculator designs:

![Design Breakdown](assets/tc_view_breakdown.png)

# Creating Views

We'll get started by creating our custom top (navigation) bar with a `UIView`.

> [info]
`UIKit` has it's own top bar called the `UINavigationBar`. To keep things simple, we'll create our own custom navigation bar. From here, when the tutorial refers to `navigation bar`, then it's refering to the custom top bar.

<!-- break -->

> [action]
Open `Main.storyboard` from your project navigator. You should see your single view controller. ![Starting Storyboard](assets/starting_storyboard.png)

Next, we'll add a `UIView` and reposition/resize it to be our custom navigation bar.

> [action]
Create a custom navigation bar by dragging an `UIView` object to your view controller and dragging it to the top of the view controller.
>
![Add Custom Navigation Bar](assets/add_custom_nav_bar.png)

It's a little hard to see now because our custom navigation bar and our view controller's root view are both white. Let's add a little constrast so both views are easier to see.

> [action]
Change the view controller's root view to off-white:
>
1. Select the view controller's root view by either clicking on it's storyboard UI or selecting it in the document outline. ![Select Root View](assets/select_root_view.png)
1. Open the _Attributes Inspector_ in the _Utilities area_. ![Open Attributes Inspector](assets/open_attributes_inspector.png)
1. Next, click on the color name or blue dropdown button beside the active color for the `Background Color` field. ![Click Background Color Dropdown](assets/click_bg_color_dropdown.png)
1. Finally, select the `Off-White` color under the _Named Colors_ subheader in the dropdown menu.

We've changed the _background color_ attribute of the root view to a different color. The `Off White` color we chose was pre-defined in our `Assets.xcasset` asset catalog.

Before we add more views or configure more properties, let's learn about the iOS coordinate system and the frame attribute of `UIView` and it's subclasses.

# iOS Coordinate System

You can think of your phone screen as a coordinate system with it's origin in the top-left corner.

<!-- TODO: show image of coordinate system with (0,0) in top-left -->

As we've previously discussed, our views can be represented as rectangles drawn on our device's screen. This rectangle can be represented by it's starting point (top-left corner of the rectangle) along with it's size (width and height). Let's look at an example:

<!-- TODO: show example of screen with a single rectangle view; add values along to x and y axes -->

<!-- break -->

> [challenge]
In the image above, what is the red view's starting point? What about it's size?

<!-- break -->

> [solution]
As you can see from the numbered X and Y axis, the view rectangle starts at the point (2, 3) in the iOS coordinate system and has a width of 3 and a height of 1.

In Swift, we can represent a coordinate point and size with the `CGPoint` (X and Y coordinate) and `CGSize` (width and height) data types respectively. Additionally, these two data types can be combined into `CGRect` data type (X, Y, width, height) that represents a rectangle represented by it's point and size properties.

In fact, each `UIView` has a property called it's `frame` that has the type of `CGRect`. You can use each view's frame property to manipulate it's position and size.

> [info]
At some point, you'll come across another view property named `bounds` that's also a `CGRect`. The `frame` of a view represents the view's rectangle in it's super view's coordinate system while it's `bound` property refers to the rectangle using the view's point as the origin of it's coordinate system. In other words, the `bounds` property of a view will always have an (x, y) of (0, 0) and retain it's size.
>
<!-- TODO: insert image of difference between frame vs bounds -->

With our new knowledge, let's properly re-position and re-size our custom navigation bar.

## Setting The Navigation Bar Rect

Looking back at our design, we can determine the `CGRect` of our custom navigation bar.

<!-- TODO: insert image of the custom navigation bar with labeled point and size -->

<!-- break -->

> [challenge]
What should the `CGRect` (x, y, width, height) of our navigation bar be?

<!-- break -->

> [solution]
It should be (0, 0, 375, 80)

Next, let's change this in storyboard.

> [action]
Open `Main.storyboard` from your project navigator. Next, select the custom navigation bar view (UIView) in _Interface Builder_. To change the view's frame, we'll need to open the _Size Inspector_ in the _Utilities area_. Change the view's frame in the size inspector to the value of the rect in the solution above.
>
<!-- TODO: show image of size inspector with correct frame values  -->

Let's see if our changes worked!

> [action]
Build and run the app in an iPhone simulator of your choice.
>
<!-- TODO: add image of clicking run button -->

You should see the custom navigation bar against your off-white root view in the simulator. Nothing fancy yet!

<!-- TODO: display image of simulator with result -->

But what happens if we run our app in a simulator with a different size screen?

# Handling Different Screen Sizes

Let's revisit our previous diagram explaining a view's frame within the iOS coordinate system.

<!-- TODO: display same image of red view in screen with X and Y axes -->

What would happen if our app was ran across multiple different screen sizes?

<!-- TODO: create image of red view with iPhone SE, iPhone 8+, iPhone X -->

As you can see, the frame of the `UIView` needs to be different for each device with a different screen size.

> [challenge]
Can you think of some ways of how we could solve this problem?

## Introducing Auto-Layout

One way we could solve different frames for each screen size is by programmatically calculating and setting each view's frame. However, that would be super messy and lead to us having to write a lot of code just make sure each view is the right size for each screen.

To solve this problem, Apple introduce a relative positioning tool called _Auto-Layout_. With _Auto-Layout_, we define constraints. Constraints allow you to define relative positioning or size rules between two views. _Auto-Layout_ will then calculate all the math and set our view's frame so that none of the constraints are broken. This allows us to build dynamic view layouts that re-position and re-shape regardless of screen size.

For example, we could give our example view the following constraints:

Top: 10 from top
Leading (Left): 4 from leading
Trailing (Right): -4 from trailing
Bottom: -60 from bottom

> [info]
Note the positive and negative values that are based on the direction of the iOS coordinate system.

<!-- TODO: add image of view using constraints above -->

Now if the screen changes, let's see how our view will react:

<!-- TODO: show image that is dynamically re-sized using constraints -->

If instead, we wanted to view to keep a fixed width or height, we can also add constraints with fixed constants:

TODO: show side by side image of fixed size with top and leading constraint in different screen sizes

<!-- TODO: this is not bad, but would like to show the faults of not using auto-layout visualized through simulator, not just through concept -->

## Setting Constraints In Interface Builder

Let's set our first constraints by changing our custom navigation bar in _Interface Builder_ to make use of constraints.

> [challenge]
What constraints should we set for our custom navigation bar?

<!-- TODO: show image of custom navigation bar with size and position -->

> [solution]
Top: 0 from top
Leading (Left): 0 from leading
Trailing (Right): 0 from trailing
Height: 80 fixed constant

Looks pretty good. Let's look at our navigation bar with these constraints across each device:

<!-- TODO: image of each device: iPhone SE, iPhone 8, iPhone 8+, iPhone X -->

Hold on. Not so fast. With the introduction of the iPhone X, the sensor housing (the top notch) requires to add some additional thought to our custom navigation bar's frame.

<!-- TODO: show image of iPhone X's top notch with labeled sizes next to regular iPhone; label status bar -->

Because the top notch, we'll need to make calculate the navigation bar's height based on the bottom of the top notch for the iPhone X.

## Safe Area

To help us handle this, Apple has provided us with the _Safe Area_. The _Safe Area_ provides us with valuable layout information to help us properly create constraints for our views. In our case, the top _Safe Area_ provides us with the bottom of the _Status Bar_ for each device:

<!-- TODO: image of top safe area relative to iPhone 8 and iPhone X safe bar; label status bar -->

Revise our original constraints, we'll need to replace our height constraint with a bottom constraint that is -80 from the top _Safe Area_. Now our navigation dynamically calculate it's layout correctly across each device.

With our correct constraints, let's set them in _Interface Builder_.

<!-- go through process with steps and images of setting constraints in IBuilder -->

# Different Types of Constraints

To properly setup constraints for our navigation bar, we've only used one type of constraints but there are many different types of constraints that you can use to build complex UI layouts. Before setting up constraints for any of our other fews, let's look at common constraints we can use to build dynamic layouts with _auto-layout_.

## Relative Position

First, let's review our relative positioning constraint. Relative positioning allows use to position a view relative to another view. That means we can place a view 10 points (iOS distance measuring unit) below the top of side of another view or even -3050 pt. from the leading (left) side of the root view.

<!-- TODO: show image of relative positioning from the example above -->

Positive and negative measurement denote which direction the constraint is measuring in, you'll need to keep in mind the iOS coordinate system.

<!-- TODO: image of iOS coordinate system and how it determines directon of constraint -->

When you're setting your relative positioning constraints, make sure you're aware of what they're relative to. It's a very common mistake to accidentally set constraints relative to something you didn't mean to. For example, you can set a view 10 pt. below another view:

<!-- TODO: image of a view 10 points below another -->

But did you set it 10 pts below the correct view?

<!-- TODO: image of a view 10 points below another view -->

And did you mean to set it 10 points below the top or bottom edge of the other view:

<!-- TODO: two side by side images of one view below the top side /bottom side of a single view -->

It's also important to take the _Safe Area_ into consideration when setting a view relative to the root view. If you're setting the top edge of your view to the top edge of the root view, you'll need to verify that you don't accidentally set it to the top edge of the _Safe Area_ instead, or vice versa.


## Constant Size (Height or Width)

As we briefly discussed earlier, it's also possible to set fixed constant constraints. These are for setting the width or height of an object to a fixed constant. For example, we can set a view to have a relative positioning constraint from the top and leading edges of the root view to 10. Then we can give our view a fixed width and height of 100 pts.

<!-- TODO: show example of view being fixed in height and position across multiple screen sizes -->

If you run into a situation where you've added your constraints but don't see your view, you might have forgotten to add certain constraints. For the previous example, if we forgot to add the height constraint, our view would look like this:

<!-- TODO: show view that doesn't show up because height constraint is 0 -->

If a view doesn't have the proper constraints set up, it's height or width value could be 0. Or it's frame could be off of the screen.

<!-- TODO: show examples of all of the incorrect cases; off the screen x 2; width is 0; height is 0 -->

## Center (With Offset) In Superview

Another positioning constraint we can use is centering within a superview. For example, if we wanted to create a button with a fixed size, we could set the following constraints:

1. Bottom: 10pt from bottom edge of superview
1. Width: 250pt
1. Height: 60pt
1. Center In Superview

Our button would look like the following across a range of devices:

<!-- TODO: Show button across different devices -->

You can also choose to offset (positive or negative to determine direction) from the superview:

<!-- TODO: show image with side by side positive and negative centering offsets -->

## Aspect Ratio

Aspect ratio constraints are also possible, where you can determine if the height is a ratio of the width. This can be useful if you want to make sure the view is always a square (1:1 aspect ratio) or if you decide that the height will always be 1/2 of the width (1:2 aspect ratio.)

If our constraints are the following:

1. Top: 25pt from top edge of root view
1. Center In Superview (root view)
1. Aspect Ratio 1:1

<!-- TODO: show image of square across different devices -->

## Equal (Ratio) To Other Constraint

The last constraint that we'll cover is the ability to set constraints relative to the ratio of another constraint. For example, we're able to set the width constraint of a view to be half of another view.

We can set the constraints as follows:

1.
1.
1.

<!-- TODO: image of constraints relative to another -->

# Setting Auto-Layout For Our View Grouping

Let's take another look at our UI group from the previous step:

<!-- TODO: add image of the group with measurements -->

We're going to go ahead setup constraints for all of our main view groups.

> [challenge]
Using the different type of constraints above, can you determine the different type of `UIView` objects we'll need along with thier constraints?

<!-- break -->

> [solution]
<!-- TODO: show image of each open with each constraint -->

## Implementing Our Constraints

We'll start implementing our constraints for each of the main views for each UI grouping. We can use the views and constraints that we came up with in the previous step.

We've already set the constraints for our custom navigation bar, let's move onto the tip input card view.

> [action]
>
Add a new view for your input card view:
<!-- TODO: insert video of dragging uiview -->
>
1. Drag a `UIView` onto the root view.
1. Set the following constraints for the tip input card:
    - top constraint 15pts below the bottom edge of the navigation bar
    - leading (left) constraint 15 from root view's leading edge
    - trailing (right) constraint 15 from root view's trailing edge
    
You'll get an _Interface Builder_ error because your new view doesn't have a set height. Don't worry about that yet, we'll fix that with another constraint once we add our output card view.

After adding our input card view, we'll also need to add an output card view. We'll follow a similar set of steps to the previous input card view that we added.

> [action]
>
Add a new view for your output card view:
>
<!-- TODO: insert video of adding uiview -->
>
1. Drag a `UIView` onto the root view below the previous input card view.
1. Set the following constraints for the tip output card:
    - top constraint 15pts below the bottom edge of the input card view
    - leading (left) constraint 15 from root view's leading edge
    - trailing (right) constraint 15 from root view's trailing edge
    - equal heights between the input card view and the output card view

Finally, let's add our reset button to finish our skeleton structure.

> [action]
Add a `UIButton` to your root view:
>
<!-- TODO: add video of adding button -->
>
1. Drag a `UIButton` below the previous output card.
1. Set the following constraints for the reset button:
    - top constraint 15pts below the bottom edge of the output card view
    - leading (left) constraint 15 from root view's leading edge
    - trailing (right) constraint 15 from root view's trailing edge
    - height constraint set to a constant of 60pts
    - bottom constraint 15pts above the bottom edge of the root view

Next, let's test that our constraints are working as expected. We can change the background color of the cards and button to a different color so it's more obvious to verify that our constraints are working.

> [action]
1. Change the background color of the tip input card, the tip output card, and the reset button to orange.
1. Build and run your app.

If all is successful, you should see the following:

<!-- TODO: show image of complete skeleton with constraints -->

We've successfully setup a skeleton for each of the view groupings of our design. In the next section, we'll work on adding individual subviews to each of the groups.


<!-- introduction to layout and ui -->

<!-- start by creating basic blocks with just preset frames -->
<!--   explain coordinate system, frame, bounds -->
<!--   what are problems with just using frames? -->
<!--   show some problems of using devices with different sizes -->

<!-- introduce auto-layout and describes how it solves this problem -->
<!--   show how you can see how auto-layout changes constraints in different devices without running on simulator -->
<!--   example of auto-layout -->
<!--   different types of constraints -->
<!--   take care of layout for 4 main groups -->
<!--   next move inside to deal with subviews for each group -->








- coordinates, frames and bounds
- why we need auto-layout
- introducting stackviews
- auto-layout vs stack views

