---
title: "UI Layout"
slug: ui-layout
---

We'll start building our app by implementing the UI in _Interface Builder_. For reference, here are the tip calculator designs:

![Design Breakdown](assets/tc_view_breakdown.png)

# Creating Views

We'll get started by creating our custom top (navigation) bar with a `UIView`.

> [info]
`UIKit` has it's own top bar called the `UINavigationBar`. To keep things simple, we'll start from scratch and create our own custom navigation bar. From this point forward, when this tutorial refers to `navigation bar`, it's referring to the custom top bar.

<!-- break -->

> [action]
Open `Main.storyboard` from your project navigator. You should see your single view controller. ![Starting Storyboard](assets/starting_storyboard.png)

Next, we'll add a `UIView` and reposition/resize it to be our custom navigation bar.

> [action]
Create a custom navigation bar by dragging an `UIView` object from the _Object Library_ to the top of the view controller. Don't worry too much about the perfect size and position for now. We'll handle that later.
>
![Add Custom Navigation Bar](assets/add_custom_nav_bar.mov)

The new `UIView` object that we just added is going to be our custom navigation bar. We'll add other subviews onto it later.

You might notice, that our custom navigation bar is a little hard to see because it's the same color as the view controller's root view: white. Let's change the color of our root view to add some contrast.

> [action]
Change the view controller's root view to off-white:
>
1. Select the view controller's root view by either clicking on it in your storyboard or selecting it in the document outline. If you don't see it in the _Document Outline_, you might have to expand the `View Controller Scene` tree. Make sure you're not selecting the custom navigation bar view by accident. ![Select Root View](assets/select_root_view.png)
1. With the root view still selected, open the _Attributes Inspector_ in the _Utilities area_. ![Open Attributes Inspector](assets/open_attributes_inspector.png)
1. Next, click on the blue dropdown button beside the active color for the `Background Color` field. ![Click Background Color Dropdown](assets/click_bg_color_dropdown.png)
1. Finally, select the `Off-White` color under the _Named Colors_ subheader in the dropdown menu. ![Select Off-White Background Color](assets/select_off_white.png)

We've changed the _background color_ attribute of the root view to a different color. The `Off White` color we chose was pre-defined in our `Assets.xcasset` asset catalog.

Before we add more views or configure more properties, let's learn about the iOS coordinate system and the frame attribute of `UIView` and it's subclasses. We'll need to learn about the iOS coordinate system to properly position and size our views.

# iOS Coordinate System

You can think of your phone screen as a coordinate system with it's origin in the top-left corner.

![iOS Coordinate System](assets/ios_coord_system.png)

As we've previously discussed, view can be represented as rectangles drawn on our device's screen. This rectangle can be represented by it's starting point (top-left corner of the rectangle) along with it's size (width and height). Let's look at an example:

![Coord Example](assets/coord_example.png)

<!-- break -->

> [challenge]
In the image above, what is the red view's starting point? What about it's size?

<!-- break -->

> [solution]
As you can see from the numbered X and Y axis, the view rectangle starts at the point (27, 48) in the iOS coordinate system and has a width of 50pts and a height of 35pts.

In Swift, we have the `CGPoint` and `CGSize` data types to represent coordinate points and sizes respectively. A `CGPoint` value is a pair of X and Y values. A `CGSize` value is a pair of width and height values.

Additionally, these two data types can be combined into `CGRect` data type (X, Y, width, height) that represents a rectangle represented by it's point and size properties.

Each `UIView` has a property called it's `frame` of type `CGRect`. You can use each view's frame property to manipulate it's position and size.

> [info]
At some point, you'll come across another view property named `bounds` that's also a `CGRect`. The `frame` of a view represents the view's rectangle in it's super view's coordinate system while it's `bound` property refers to the rectangle using the view's top-left corner as the origin of it's coordinate system. In other words, the `bounds` property of a view will always have an (x, y) of (0, 0) and retain it's size.
>
![Frame vs Bounds](assets/frame_vs_bounds.png)

With our new knowledge, let's properly re-position and re-size our custom navigation bar.

## Setting The Navigation Bar Rect

Looking back at our design, we can determine the `CGRect` of our custom navigation bar.

> [challenge]
What should the `CGRect` (x, y, width, height) of our navigation bar be?
>
![Navigation Bar Dimensions](assets/nav_bar_dimensions.png)

<!-- break -->

> [solution]
The _frame_ of our navigation bar is (0, 0, 375, 105).

Let's change our current nav (navigation) bar's frame in storyboard.

> [action]
In `Main.storyboard` perform the following:
>
1. Select the custom navigation bar view (UIView) in _Interface Builder_
1. With the nav bar selected, open the _Size Inspector_ in the _Utilities area_.
1. Find the view's `Frame Rectangle` fields. Change the X, Y, Width and Height values in the size inspector to the value of the rect in the solution above.
>
![Fixed Navigation Bar](assets/fix_nav_bar_rect.png)

Let's see if our changes worked!

> [action]
Build and run the app in the iPhone 8 simulator by clicking the run button in the toolbar.
>
![Click Run](assets/click_run.png)

You should see the custom navigation bar against your off-white root view in the simulator. Nothing fancy yet!

![Fixed Nav Bar Simulator](assets/fixed_nav_bar_simulator.png)

But what happens if we run our app in a simulator with a different size screen?

# Handling Different Screen Sizes

Let's revisit our previous diagram explaining a view's frame within the iOS coordinate system.

![Nav Bar Dimensions](assets/nav_bar_dimensions.png)

What would happen if our app was ran across multiple different screen sizes?

![Fixed Nav Bar With Different Screen Sizes](assets/fixed_nav_bar_diff_screens.png)

As you can see, the frame of the `UIView` needs to be different for each device with a different screen size.

> [challenge]
Can you think of some ways of how we could solve this problem?

## Introducing Auto-Layout

One way we could solve different frames for each screen size is by programmatically calculating and setting each view's frame. However, that would be super messy and lead to us having to write a lot of code just make sure each view is the right size for each screen.

To solve this problem, Apple created a relative positioning tool called _Auto-Layout_. With _Auto-Layout_, we define constraints.

Constraints are rules where you can define the relative positioning or size between two views. _Auto-Layout_ will then calculate all the math and set our view's frame so that all of the constraints (rules) are followed. This allows us to build dynamic view layouts that re-position and re-shape for any screen size.

For example, we could give our example view the following constraints:

- Top: 20pts from Super View (Root View) Top Edge
- Leading (Left): 40pts from  Super View (Root View) Leading Edge
- Trailing (Right): -80pts from  Super View (Root View) Trailing Edge
- Bottom: -380pts from bottom

> [info]
Note the positive and negative values that are based on the direction of the iOS coordinate system.

![Constraints Example](assets/constraints_example.png)

Now if the screen changes, let's see how our view will react:

![Constraints Example](assets/constraints_example_diff_screens.png)

See how auto-layout calculates the view's frame based on our constraints for each different screen size?

If instead, we wanted give the view a fixed width or height, we can also add constraints as fixed constants. Let's give our example view a new set of constraints:

- Top: 20pts from Super View (Root View) Top Edge
- Leading (Left): 40pts from  Super View (Root View) Leading Edge
- Width: 150pts
- Height: 200pts

![Fixed Size Constraints Example](assets/fixed_size_constraint_example.png)

Auto-layout and constraints give us an easy way to build dynamic view layouts for any iOS device.

## Determining Constraints

Let's set our first constraints by changing our custom navigation bar in _Interface Builder_ to make use of constraints.

> [challenge]
What constraints should we set for our custom navigation bar?

<!-- TODO: show image of custom navigation bar with size and position -->

> [solution]
Our nav bar would have the following constraints:
>
- Top: 0 from Super View (Root View) Top Edge
- Leading (Left): 0 from Super View (Root View) Leading Edge
- Trailing (Right): 0 from Super View (Root View) Trailing Edge
- Height: 105 fixed constant

Looks pretty good. Let's look at our navigation bar with these constraints across each device:

![Nav Bar With Fixed Height Constraint](assets/nav_bar_w_fixed_height_constraint.png)

Hold on. Not so fast. With the introduction of the iPhone X, the sensor housing (the top notch) requires to add some additional thought to our custom navigation bar's frame.

![Nav Bar Fixed Height Problem](assets/nav_bar_fixed_height_problem.png)

Because the top notch, we'll need to make calculate the navigation bar's height based on the bottom of the top notch for the iPhone X.

To help us handle this, Apple has provided us with the _Safe Area_.

## Safe Area

The _Safe Area_ provides us with valuable layout information to help us properly create constraints for our views. In our case, the top _Safe Area_ provides us with the bottom of the _Status Bar_ for each device:

![Safe Area](assets/safe_area.png)

Revise our original constraints, we'll need to replace our height constraint with a bottom constraint that is -85 from the top _Safe Area_. Now our navigation dynamically calculate it's layout correctly across each device.

![Nav Bar Correct Height](assets/nav_bar_correct_height.png)

It might be a little hard to see, but the height of the nav bar is slightly bigger for the iPhone X because of it's top notch.

With our correct constraints, let's set them in _Interface Builder_.

## Setting Our First Constraints

Let's set our constraints in _Interface Builder_. First we'll start by adding our top, leading (left) and trailing (right) constraints.

> [action]
Open `Main.storyboard` from your _Project Navigator_. Select your custom navigation bar view (`UIView`) and add the following constraints:
>
![Set Nav Bar Edge Constraints](assets/edge_constraints_nav_bar.mov)
>
With our nav bar view selected, we click on the _Add New Constraints_ button and set each of the edge constraints:
>
- _Top Edge_ of navigation bar 0pts to _Top Edge_ of root view
- _Leading (Left) Edge_ of navigation bar 0pts to _Leading (Left) Edge_ of root view
- _Trailing (Right) Edge_ of navigation bar 0pts to _Trailing (Right) Edge_ of root view

Currently, our navigation bar has a incomplete set of constraints. We haven't added a constraint to define the view's height yet. If we run the app now, we won't see our navigation bar because it's height will be 0. Xcode and _Interface Builder_ try to warn us of this:

![Constraint Errors](assets/constraint_errors.png)

You'll notice above:

1. a red error arrow that lists missing constraints in your document outline
1. red highlights around the custom nav bar view in your storyboard
1. a warning in the Xcode project status bar

Let's add the final constraint to define the nav bar's height.

> [action]
Add a constraint from the bottom edge of the nav bar to the top edge of the _Safe Area_: ![Nav Bar Height Constraint](assets/nav_bar_safe_area_constraint.mov)
>
To add the constraint in the video, follow the steps below:
>
1. Select the navigation bar view (`UIView`) in the _Document Outline_.
1. With the nav bar selected, hold down the control button (ctrl) and click-drag from the navigation bar view to the _Safe Area_ view in your _Document Outline_.
1. Once you let go, you'll see a pop-up with the options to add a new constraint. Select _Vertical Spacing_. This will set a vertical spacing constraint from the top edge of our navigation bar to the top edge of the _Safe Area_.
1. (Optional) If you'd like to adjust the constraint, you can click on it and adjust it's values in the _Size Inspector_.

Congrats, we've added our first set of constraints to our navigation bar. Try running your app in multiple different simulators and see if our navigation bar properly adjusts it's frame to each device.

![Auto-Layout Navigation Bar Multiple Devices](assets/dynamic_nav_bar_diff_devices.png)

It works! Next, we'll dive deeper into _auto-layout_ and the different kind of constraints that are available to use.

# Different Types of Constraints

To properly setup constraints for our navigation bar, we've only used one type of constraints but there are many different types of constraints that you can use to build complex UI layouts. Before setting up constraints for any of our other fews, let's look at common constraints we can use to build dynamic layouts with _auto-layout_.

## Relative Position

First, let's review our relative positioning constraint. Relative positioning allows use to position a view relative to another view. That means we can place a view 10 points (iOS distance measuring unit) below the top of side of another view or even -3050 pt. from the leading (left) side of the root view.

<!-- TODO: show image of relative positioning from the example above -->

Positive and negative measurement denote which direction the constraint is measuring in, you'll need to keep in mind the iOS coordinate system.

<!-- TODO: image of iOS coordinate system and how it determines directon of constraint -->

When you're setting your relative positioning constraints, make sure you're aware of what they're relative to. It's a very common mistake to accidentally set constraints relative to something you didn't mean to. For example, you can set a view 10 pt. below another view:

<!-- TODO: image of a view 10 points below another -->

But did you set it 10pts below the correct view?

<!-- TODO: image of a view 10 points below another view -->

And did you mean to set it 10pts below the top or bottom edge of the other view:

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

Let's take an other look at our design:

![TC Design](assets/tc_design_reference.png)

Next, we're going to setup the main views (and their constraints) for each UI group. In other words, we'll add the objects and constraints below:

![UI Groups With Dimensions](assets/ui_groups_with_dimensions.png)

## Implementing Our Constraints

We've already finished implementing the foundation for our custom navigation bar. We'll repeat a similar process for each of our remaining UI groups.

With our nav bar complete, let's move on to implementing the tip input card.

### Input Card View

> [action]
Open `Main.storyboard`. Add a new `UIView` and set the following constraints:
>
![Add Input Card With Constraints](assets/add_input_card_w_constraints.mov)
>
Step by step:
>
1. Drag a `UIView` from the _Object Library_ onto the root view.
1. Click the `Add New Constraints` button at the bottom right corner of the _Interface Builder Editor_ window.
1. Set the following constraints:
    - (Input Card) _Top Edge_ 24pts from Nav Bar _Bottom Edge_
    - (Input Card) _Leading (Left) Edge_ 15pts from Super View (Root View) _Leading (Left) Edge_
    - (Input Card) _Trailing (Right) Edge_ 15pts from Super View _Trailing (Right) Edge_

At this point, you'll see an _auto-layout_ error because your new (input card) view is missing a height constraint. Ignore this warning for now, we'll fix this soon.

Next, we'll add our output card and it's constraints.

### Output Card View

> [action]
In storyboard, add a new `UIView` and set the following constraints:
>
![Add Output Card With Constraints](assets/add_output_card_w_constraints.mov)
>
Step by step:
>
1. Drag a `UIView` from the _Object Library_ onto the view controller's root view, below the input card.
1. Click the `Add New Constraints` button at the bottom right corner of the _Interface Builder Editor_ window.
1. Set the following constraints:
    - (Output Card) _Top Edge_ 24pts from Input Card _Bottom Edge_
    - (Output Card) _Leading Edge_ 15pts from Super View _Leading Edge_
    - (Output Card) _Trailing Edge_ 15pts from Super View _Trailing Edge_

We'll also add an equal height constraints between both input and output card views.

> [action]
Add an equal heights constraint between both input and output cards:
>
![Add Equal Heights Constraint](assets/add_cards_equal_height_constraint.mov)
>
Step by step:
>
1. Select the output card view.
1. With the output card selected, hold down shit and then click on the input card view. This will allow you to select both card views.
1. Click the `Add New Constraints` button at the bottom right corner of the _Interface Builder Editor_ window.
1. In the popup prompt, select `Equal Heights` and add the selected constraint.

Xcode should still show an _auto-layout_ error because we haven't added enough constraints for it determine the height of each card view. Ignore this warning for now, this will be fixed once we add our reset button.

### Reset Button

> [action]
In storyboard, add a new `UIButton` and set the following constraints:
>
![Add Reset Button](assets/add_reset_button_w_constraints.mov)
>
Step-by-step:
>
1. Drag a `UIButton` from the _Object Library_ onto the view controller's root view, below the output card.
1. Click the `Add New Constraints` button at the bottom right corner of the _Interface Builder Editor_ window.
1. Set the following constraints:
    - (Reset Button) _Top Edge_ 24pts from Output Card _Bottom Edge_
    - (Reset Button) _Leading Edge_ 15pts from Super View _Leading Edge_
    - (Reset Button) _Trailing Edge_ 15pts from Super View _Trailing Edge_
    - (Reset Button) _Bottom Edge_ 24pts from Super View _Bottom Edge_
    - (Reset Button) _Height_ of 60pts

By default, our button has a clear background color. To make our reset button easier to see, let's change it's background color from `Clear` to `tcDarkBlue`.

> [action]
Change the _Background color_ of the reset button:
>
![Set Reset Button Background Color](assets/set_reset_button_bg_color.mov)
>
Step-by-step:
>
1. Select the _Reset Button_.
1. With the _Reset Button_ selected, navigate to the _Attributes Inspector_ in the _Utilities area_.
1. Scroll down until you find the `Background` field. This field allows you to set the button's background color.
1. Locate the blue dropdown button and set the button's background color from `Clear` to `tcDarkBlue`.

<!-- break -->

> [info]
Our _auto-layout_ warning is gone! After adding our reset button and it's constraints, _auto-layout_ can calculate the height of each input/output card using the equal heights constraint.

We've finished implementing the main view for each of our respective UI groups. For each group, we added the appropriate `UIView` object and set it's corresponding constraints.

Before moving on, let's test that everything looks as expected.

# Testing Our Constraints

To catch bugs or missteps early, it's always good to build and run your code often. Let's go ahead and do that now to test that our constraints are working correctly.

> [action]
In the toolbar, click the _Run_ button.

If everything goes as expected, you should see the following in your simulator:

![Finished UI Groups](assets/finished_ui_groups.png)

Try running our project on different simulators. You'll notice that our view dynamically adjust and re-size for any screen size:

![Finished UI Groups Different Devices](assets/finished_ui_groups_diff_devices.png)

## Conclusion

In this section, we learned about how to layout our UI; first with frames and later with _auto-layout_. We learned about constraints and their importance in building dynamic view layouts for multiple devices. And finally, we put our knowledge into practice by implementing a scaffolding for our tip calculator design.

In the next section, we'll build off of our UI by fully implementing and styling each of our UI groups.
