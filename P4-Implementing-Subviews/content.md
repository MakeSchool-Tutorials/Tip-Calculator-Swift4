---
title: "Implementing Subviews"
slug: implementing-subviews
---

In the previous section, we've built a skeleton of the main scaffolding of our UI.

<!-- TODO: insert image of progress so far -->

In this section, we'll finish implementing each of our _view groups_:

1. Custom Navigation Bar
1. Tip Input Card
1. Tip Output Card
1. Reset Button

For each group, we'll need to:

1. Add the correct UIKit objects
1. Identify and set _auto-layout_ constraints for each view object
1. Create the appropriate `IBOutlets` and `IBActions` for each view object

By implementing and connecting our UI, we'll set a strong foundation for implementing our future tip calculator code.

Let's get started by completing our navigation bar.

# Custom Navigation Bar

We've already build the base view of our custom navigation bar using a `UIView`:

<!-- TODO: show image of skeleton navigation bar -->

To finish our navigation bar, we'll need to first add the remaining subviews: a `UILabel` and `UISwitch`.

<!-- TODO: insert design of nav bar -->

Let's start by adding the title label.

## Title Label

Before we add our title label in our storyboard, let's first think all of constraints we'll need to create our layout.

> [challenge]
Using the design below, identify each of the _auto-layout_ constraints for your title label.
>
<!-- TODO: insert design of nav bar with distances and sizes of each element -->

<!-- break -->

> [solution]
Check that you have all of the necessary constraints for the title label:
>
<!-- TODO: add image of title label with all necessary constraints -->
>
Each of the constraints listed out:
>
- leading from leading edge
- how to have centered vertically from safe area guide?

After verifying that your title label constraints are correct, let's implement our new label in storyboard.

> [action]
Open storyboard and implement your navigation bar title label:
<!-- TODO: add movie of dragging title label onto storyboard nav bar and setting constraints -->
>
Let's break down the video step-by-step:
>
1. Drag a `UILabel` from the _Object Library_ on top of the custom nav bar view (`UIView`) in your view controller.
1. Add the constraints we previous identified to our label:
    - leading from leading
    - centered vertically
    
We've successfully added the constraints to our navigation bar title label. 

<!-- TODO: show image of title label with correct constraints but without styling -->

Next, we'll style it with the correct attributes so it matches our design.

> [action]
In your storyboard, select your nav bar title label and open the _Attributes Inspector_ in the _Utilities area_. Using the _Attributes Inspector_, find each of the view object propertoes and change their values to the following:
    - font and font size
    - font color
    - text
    
After you're done, your new title label should look like the following:

<!-- TODO: insert image of just title label (but stylized) in nav bar -->

Next, let's move on to our `UISwitch`:

## Light/Dark Mode (Theme) Switch

We'll finish our navigation bar by implementing our switch. Again, let's start by figuring out all of the constraints we'll need for our `UISwitch`.

> [challenge]
Using the design below, identify each of the _auto-layout_ constraints for your switch.
>
<!-- TODO: insert design of nav bar with distances and sizes of each element -->

<!-- break -->

> [solution]
For our `UISwitch`, we'll need the following the constraints:
<!-- TODO: add image of all of the switch constraints -->
>
Each of the constraint listed:
    - trailing to trailing edge
    - center to center of label





Next, we'll add our switch and it's constraints in storyboard.

> [action]
TODO: add video of adding switch and it's constraints
>
1. Drag a `UISwitch` from the _Object Library_ to your view controller.
1. Size and position so that it looks relatively correct (we'll add constraints next but this will make it easier)
1. Add the following constraints to your `UISwitch`
    - leading
    - leading
    - leading
1. Select your switch in storyboard and open the _Attributes Inspector_ in the _Utilities area_. Find the `tint color` attribute and change it to `Seafoam Green`.

## Adding Our IB Connections

We've almost finished building our custom navigation bar. Before we move on to our tip input card, we'll need to make our `IBOutlets` and `IBActions` so that we can interact with our storyboard objects with code.

For review, `IBOutlets` create a reference in our view controller so that we can write code that interacts with our `UILabel` and `UISwitch` objects. We'll need this later to implement our tip calculator logic for theming.

First, let's create a `IBOutlet` for our title label.

> [action]
<!-- TODO: add video of adding IBOutlet for title label -->
>
1. add step 1
1. add step 2
1. add step 3
1. add step 4

Next, we'll create an `IBAction` for our `UISwitch`. Remember `IBAction` can be triggered by `UIControlEvents`. In our case, we'll be able to trigger an event whenever the switch is trigggered.

> [action]
Let's create an `IBAction` for our switch:
<!-- TODO: add video for adding the ibaction for our switch -->

<!-- break -->

> [info]
you can also abstract this into a custom view object, but we won't do that for sake of simplicity in this tutorial

We've now finished building out the UI for our custom navigation bar. Now that you're done, your UI should look like this:

TODO: add image of storyboard

> [action]
Build and run your app. If everything has gone well, you should see the following:
>
<!-- TODO: add image of simulator successfully running -->

# Input Card View

Next, we'll move onto the same process for our input card view. First let's review our design for our input card:

<!-- TODO: add image of input card design -->

In our input card view, we'll need to add two labels for each respective user input in addition to our text field and segmented control.

To add constraints to the subviews on our tip input card, we'll introduce a new layout tool called: `UIStackView`.

## Stack Views

`UIStackView` allow us to create horizontal or vertical stacks of views. This can be especially useful for easily organizing the layout of elements without need to use a complex set of constraints.

<!-- TODO: add image of horizontal and vertical stack view side by side -->

For our case, we'll use stack views to easily group our input subview objects.

Our first `UIStackView` will group together our first label and it's respective text field.

<!-- TODO: show image of stack view grouping together label element and text field element -->

> [action]
Create a stack view using your first label and it's respective text field.
>
<!-- TODO: add video of adding label and text field and grouping into a stack view -->
>
In the video above, we do the following:
>
1. Add a `UILabel` and `UITextField` from the _Object Library_ side-by-side.
1. Make sure both objects are positions side-by-side horizontally. Thier relative positioning will determine the direction of the stack view created in storyboard.
1. Create the create stack view button in _Interface Builder_ to create your stack view.
1. Select your stack view in your document outline and open the _Attributes Inspector_. Then set the spacing between each object to 30.
1. Select the text field in your stack view and set a constant width constraint of 150. <!-- TODO: check this -->

Great! We've finished our first stack view. We'll need to create a second on for our second grouping of user input.

<!-- TODO: show image of tip percent segmented control with stack view outline -->

> [action]
Create a stack view using another label and a segmented control:
>
<!-- TODO: add video of adding label and segmented control and grouping into a stack view -->
>
In the video above, we do the following:
>
1. Add a `UILabel` and `UISegmentedControl` from the _Object Library_ side-by-side.
1. Make sure both objects are positions side-by-side horizontally. Thier relative positioning will determine the direction of the stack view created in storyboard.
1. Create the create stack view button in _Interface Builder_ to create your stack view.
1. Select your stack view in your document outline and open the _Attributes Inspector_. Then set the spacing between each object to 30.
1. Select the segmented control in your stack view and set a constant width constraint of 150. <!-- TODO: check this -->

Next, we'll create a vertical stack view from our two horizontal stack views.

> [action]
<!-- TODO: add video of selecting both previous stack views and create stack view -->
>
We use both of our previous stack views and create a vertical stack views from both elements. `UIStackView` is another view object that automatically generates layout constraints for each of the view objects within the stack view.

<!-- TODO: maybe talk more about stack view, what it is, what it does and it's properties (subview array) -->

Your tip input card view should now look like the following:

<!-- TODO: show image of stack view without constraints -->

> [info]
In this section, we used stack views to implement our layout. It's also possible to just use auto-layout constraints, but that would require a lot more work. Try to think of the constraints you'd need to use create the same layout. Stack views are especially useful turning layouts that require a complex set of constraints into groups of `UIStackView` objects that apply constraints to all of the subviews within the stack view automatically.

Finally, we'll need to center our stack view without our input card view. We can use the set in container view constraint for this task.

> [action]
<!-- TODO: add video of centering within card view -->
>
1. Select outside stack view and center horizontally in container view.
1. Select outside stack view and center vertically in container view.

## Adding Our IB Connections

Last, to complete this group, we'll also need to add our `IBOutlets` and `IBActions`.

> [action]
Create the following `IBOutlets` and `IBAction` for each of the views in the tip input card:
1. Tip Input Card (`UIView`)
1. Bill Amount Label (`UILabel`)
1. Bill Amount Text Field (`UITextField`)
1. Tip Percent Label (`UILabel`)
1. Tip Percent Segmented Control (`UISegmentedControl`)
1. Tip Percent Segmented Control Value Changed IBAction

<!-- TODO: add custom textfield, but don't do that here -->

# Output Card View

We'll implement our layout similar to the way we implemented our input card view subviews: with stack views.

> [challenge]
In the same manner as the previous view grouping, implement the view layout for your output card view. Start by identifying the different `IBOutlets` and/or `IBActions` you'll need for your output card, and then each one at a time.

<!-- break -->

> [solution]
<!-- TODO: add video creating output card view and subviews -->
You can create your output card view with the following steps:
1. Add two labels side by side. Add them into a stack view.
1. Add two labels side by side. Add them into a stack view below the first group.
1. Create a vertical stack view with the two previous stack views.
1. Set the attributes for the following labels.
    - do this
    - do this
    - do this
1. Set the outside stack view to be center in the container view horizontally and vertically.
1. Create a `IBOutlet` for each respective label. Name them the following:
    - tip amount title label
    - tip amount label
    - total bill amount title label
    - total bill amount label

When finished, build and run your app. This is what your app should look like this far.

# Reset Button

We won't need to add any subviews to our reset button. Instead, we'll just need to configure some of our button's attirbutes using the _Attributes Inspector_ and create it's respective IB connections.

First, let's configure our button's style.

> [action]
Select the button in your storyboard and open the _Attributes Inspector_. Change the following attributes:
    - background color
    - text color
    - title font size

## Creating IB Connections

Next, we'll need to create our IB connections.

> [challenge]
First, let's create our `IBOutlet` for our button.

> [solution]
You can create a `IBOutlet` by ctrl-dragging like the following:
<!-- TODO: show video of creating IBOutlet with button -->

To wrap up we'll also need to create an `IBAction`:

> [challenge]
Create an `IBAction` for the `TouchUpInside` event for your reset button.

> [solution]
TODO: show video creating IBAction for reset button


Now let's test that it's work. Put a print statement to test that it's working.

Build and run. If working successfully, you should be able to see the following.

# Conclusion

Although we're not completely done with our styling, we'll first implement our logic for our tip calculator. In this section, we implemented Let's move onto the next section.

<!-- below is scaffolding; remove before production -->

# Dealing Layers (maybe move this to another section)
## shadows
## rounded corners



header

tip input
  using auto-layout
  using stack views

tip output
  challenge using stack views

maybe move this to next section
  - add shadow to nav bar
  - add rounded corners to cards and button


