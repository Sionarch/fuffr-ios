# Fuffr Tutorial

Start developing cool apps for the Fuffr! This tutorial gets you going.

Current release of Fuffr is targeted for iOS devices. Android support will follow.

## What you need

To develop Fuffr apps, you need:

* Basic knowledge of Objective-C and iOS development.
* A Mac running a recent version of Xcode.
* An iPhone 5/5C/5S running iOS 7 or higher. (You can also use iPhone 4S, but Fuffr is designed for the iPhone 5 form factor.)
* Please note that you cannot run Fuffr apps in the iOS Simulator, because it lacks support for BLE (Bluetooth Low Energy) needed to connect to Fuffr.
* The source code for the Fuffr library and the example apps (available on GitHub).
* Fuffr hardware (contact hello@fuffr.com for information on availability).

## Get the source code

You have two options:

Install Git and clone the Fuffr GitHub repository using the command:

    git clone git@github.com:fuffr/fuffr-code.git

Download the code as a zip-file by clicking **Download ZIP** on [GitHub](https://github.com/fuffr/fuffr-code).

## Run the example apps

The quickest way to get started is by exploring the example apps:

**FuffrHello** - This is a minimal example that shows how to control two circles using the left and right sensor arrays. This example uses a single touch interaction style for each side.

**FuffrDots** - A minimal example that illustrates how to access multi touch data. For each touch point, a circle is drawn.

**FuffrMap** - Example app that show how to navigate Apple maps with Fuffr. Use right side finger to pan (scroll) across the map, and the left side finger to zoom the map with up/down movements.

**FuffrJS** - This example demonstrates how to interface with JavaScript and pass touch events to an app written in HTML/JS.

## Add the FuffrLib to your own app

**You only need to read the following if you wish to use Fuffr with your existing apps. The examples that comes with Fuffr are already set up properly.**

**FuffrLib** is an Xcode library you can add to your own iOS applications.

Follow these steps to include the library into you app:

Open your application in Xcode.

Make sure that no other application that includes FuffrLib is open in Xcode.

From the Finder, drag and drop the file **FuffrLib.xcodeproj** into the Xcode Project Navigator.

Select your project (the topmost entry) in the Project Navigator and select tab **Build Settings**.

Under **Search Paths/Header Search Paths**, enter the following in the Debug and Release fields (make sure to include also the quote marks):

    "$(TARGET\_BUILD\_DIR)/usr/local/lib/include" "$(OBJROOT)/UninstalledProducts/include" "$(BUILT\_PRODUCTS\_DIR)"

Under **Linking/Other Linker Flags**, enter the following in the Debug and Release fields:

    -ObjC

Next select tab **Build Phases**.

Under **Target Dependencies**, click **+** (plus) and add **FuffrLib**.

* Under **Link Binary With Libraries**, click **+** (plus) and add **libFuffrLib.a**.

For more information about using libraries, see the [iOS Developer Library Documentation](https://developer.apple.com/library/ios/technotes/iOSStaticLibraries/Articles/configuration.html#//apple_ref/doc/uid/TP40012554-CH3-SW1).

## The Fuffr API

Here is an overview of classes and methods provided by the Fuffr API.

### Class overview

The following classes contain functionality needed by most applications:

Class **FFRTouchManager** ([FuffrLib/FFRTouchManager.h](https://github.com/fuffr/fuffr-ios/blob/master/FuffrLib/FuffrLib/Touch/FFRTouchManager.h)) contains methods for connecting to Fuffr and for observing touch events. This is a singleton, the system creates and maintains the single instance of this class.

Class **FFRTouch** ([FuffrLib/FFRTouch.h](https://github.com/fuffr/fuffr-ios/blob/master/FuffrLib/FuffrLib/Touch/FFRTouch.h)) represents touches, with information like the touch coordinate and which side that generated the event.

Enum **FFRCaseSide** ([FuffrLib/FFRTouch.h](https://github.com/fuffr/fuffr-ios/blob/master/FuffrLib/FuffrLib/Touch/FFRTouch.h)) has constants that represent each side: **FFRSideTop**, **FFRSideBottom**, **FFRSideLeft**, **FFRSideRight**.

### Connecting to Fuffr

The first step in an app is to connect to Fuffr. The app uses BLE (Bluetooth Low Energy) to communicate with Fuffr.

This is how to get a reference to the **FFRTouchManager** and connect:

	FFRTouchManager* manager = [FFRTouchManager sharedManager];

    [manager
        connectToFuffrNotifying: self
        onSuccess: @selector(fuffrConnected)
        onError: nil];

### Registering touch observers

To observe touch events, the application registers methods with the **FFRTouchManager**. Here is an example that registers touch methods for the right side:

    [manager
        addTouchObserver: self
        touchBegan: @selector(touchRightBegan:)
        touchMoved: @selector(touchRightMoved:)
        touchEnded: @selector(touchRightEnded:)
        side: FFRSideRight];

### Receiving touch events

The methods registered to receive touch events takes one parameter, an **NSSet** that contains one or more **FFRTouch** objects. This is an example that logs the normalized coordinates of each touch instance:

    - (void) touchRightBegan: (NSSet*)touches
    {
		for (FFRTouch* touch in touches)
		{
			NSLog(@"Touch norm.x: %.02f norm.y: %.02f",
		        touch.normalizedLocation.x,
		        touch.normalizedLocation.y);
		}
    }

## Let us know what you think!

We would love to hear about the Fuffr apps you create. Let us know about your work, what you think Fuffr, the API, and please report any bugs you might encounter.

Contact: hello@fuffr.com