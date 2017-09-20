---
title: "Getting-Started""
slug: getting-started
---

In this section, we'll get introduced to our new Xcode project and start working on implementing our tip calculator design.

We'll get started by downloading a Xcode starter project. You can download the `.zip` file by [clicking here.](https://github.com/ocwang/TipCalculatorStarter/archive/master.zip)

# In Your Starter Project

After downloading the starter project, open the project in Xcode. You should see something the following:

![Open Starter Project](assets/open_starter_project.png)

Build and run the current starter project in a simulator or iPhone device of your choice.

> [action]
Run the new starter project to make sure there are no compilation errors. If all goes well, your project should run successfully and display an empty white screen (we haven't built anything yet!)
>
![Empty Screen](assets/empty_screen.png)

Next, let's take a look inside our starter project.

## View Controller

<!-- TODO: considering adding a section about directory layout? -->

First, we'll start with our view controller. 

> [action]
Open the `ViewController.swift` source file. You should see the following code:
>
```
class ViewController: UIViewController {
>
    // MARK: - View Lifecycle
>
    override func viewDidLoad() {
        super.viewDidLoad()
    }
}
```

Just boilerplate code, nothing is happening yet. We'll need to write our tip calculator logic in here later.

> [action]
Next open `Main.storyboard`. You'll see the following:
>
![Starting Storyboard](assets/starting_storyboard.png)

You can notice in the class inspector, the `ViewController.swift` source code is paired with the View Controller object in your `Main.storyboard`.

![Storyboard View Controller Class](assets/storyboard_vc_class.png)

## XCAssets

Next we'll take a look at our project's assets.

> [action]
Open `Assets.xcassets` to see your project assets. ![Project Assets](assets/project_assets.png)

In your asset catalog, you should see an app icon that's already set for you and two folders containing pre-defined color sets for the different color themes we'll implement later.

To pair with our custom colors in our asset catalog, your project also contains a `UIColor` extension that allows you to access each of the app's colors through code.

> [action]
Open `UIColor+TC.swift` from your project navigator. You should see the following:
>
```
import UIKit.UIColor
>
extension UIColor {
>
    // MARK: Theme Colors
>
    static var tcDarkBlue: UIColor {
        return UIColor(named: "tcDarkBlue")!
    }
>
    static var tcOffWhite: UIColor {
        return UIColor(named: "tcOffWhite")!
    }
>
    static var tcHotPink: UIColor {
        return UIColor(named: "tcHotPink")!
    }
>
    static var tcCharcoal: UIColor {
        return UIColor(named: "tcCharcoal")!
    }
>
    static var tcAlmostBlack: UIColor {
        return UIColor(named: "tcAlmostBlack")!
    }
>
    static var tcMediumBlack: UIColor {
        return UIColor(named: "tcMediumBlack")!
    }
>
    static var tcBlueBlack: UIColor {
        return UIColor(named: "tcBlueBlack")!
    }
>
    static var tcSeafoamGreen: UIColor {
        return UIColor(named: "tcSeafoamGreen")!
    }
>
    static var tcBlack: UIColor {
        return UIColor(named: "tcBlack")!
    }
>
    static var tcWhite: UIColor {
        return UIColor(named: "tcWhite")!
    }
}
```

This file pairs each of the class variables to the custom color sets defined in our asset catalog.

In your code, you can access each respective color through the `UIColor` class variable:
>
```
let selectedColor = UIColor.tcSeafoamGreen
```

Notice that we prefix each of our custom colors with `tc` to avoid namespace conflicts and make our colors easier to fix with Xcode autocomplete.

> [info]
In this project, your custom colors have been defined in your asset catalog, however this is just one of many ways for defining custom colors. You can also create custom colors programmatically or through _Interface Builder_.

## App Delegate

To wrap up our project tour, we'll take a look at our app delegate.

> [action]
Open `AppDelegate.swift` from your project navigator:
>
```
import UIKit
>
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
>
    var window: UIWindow?
>
    // MARK: - App Lifecycle
>
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        return true
    }
}
```

We'll cover this file briefly because we won't change the app delegate in this app. 

Each iOS Xcode project must have an `AppDelegate.swift` file which is responsible for the app's lifecycle. The app delegate specifies what happens when app lifecycle events are triggered. Common example events are app launch, receiving a push notification, app termination, etc.

In our tip calculator's app delegate, we don't add any code other than the boilerplate `application(_:didFinishLaunchingWithOptions:)` method that comes by default in each Xcode project template. 

We've just taken a look at the files in our new Xcode starter project. We'll build on this project to create our final tip calculator. Let's get started by diving into storyboard and creating our views for our UI.

