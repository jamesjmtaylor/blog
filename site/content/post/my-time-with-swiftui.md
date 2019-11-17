---
title: My time with SwiftUI
date: '2019-11-17T07:32:20-08:00'
---
<img style="float: left; margin:0 2em 1em 0; width: 25%" src="/img/blog/swiftui.png"/> Back in September I got a notice from Apple that one of my personally developed apps, the <a href=" https://apps.apple.com/us/app/wrd-calculator/id1148696352?ls=1"> WRD Calculator</a>, would be removed from the store in one month if I didn't update it.   Apple also released MacOS Catalina that month.  Given this conjunction of events my mission was clear: update my Macbook Pro's OS and rebuild my app using SwiftUI (which requires Catalina to develop with).  Working mornings and weekends I hacked a new app, converting what had been an Ionic abomination of HTML, CSS, and Typescript, into a purely native mobile application.  Along the way I learned a lot about SwiftUI's strengths and weaknesses. 

One of the biggest shocks for me was that in SwiftUI there are no longer ViewControllers.  You can still access lifecycle methods through the SceneDelegate, but other than that the traditional ViewController logic is absent.

SwiftUI still has a lot of issues with compositing multiple elements.  A view cannot have more than 10 child views.  If you ever get an odd error message that is “Unable to infer complex closure return type; add explicit type to disambiguate”, thenviews into either VStack or HStack subviews.  Alternatively you can create an entirely separate and new view that you embed into the initial view.  Finally, if all else fails, try restarting Xcode.

Under Combine, if you have an Observable object with a @Published property, updating that property will notify the UI to update with the new value.  It will also break any Binding<*> object references that you created by prefixing objects with the $ sign.  

SwiftUI TextFields need a Binding<*> object in order to notify the VM that they have been updated by the user, otherwise the field will be populated by the initial VM value, but will never update with the user input updates

SwiftUI is not like Cocoa. It is not event-driven, and there is no communication from one interface object to another. There is only data. Data flows thru state variables. It flows up by a state variable's binding and down by a state variable's value.

When creating your SwiftUI view models you can use either \`@EnvironmentObject\` or \`@ObservedObject\`. In the former case the object is available throughout the environment.  In the latter it’s available to only the object that it’s passed into. To create an EnvironmentObject you label the View’s view model property as an \`@EnvironmentObject\` and pass in an instance using the View’s ‘environmentObject()\` function. To create an ObservedObject you label the View’s view model property as an \`@ObservedObject\` as a construction argument.  In both cases the view model itself must conform to the ObservableObject protocol, use \`@Published` properties for fields that will need to update with user input, and use computed properties for those that rely on those fields for the output.  Generally ObservedObject is a more self-contained solution, and therefore prefferred.

The TabView accent color does not  affect the color of tab icons that you provide yourself through \`Image(“myImage”)\`.  Instead you can only use system provided images i.e. \`Image(systemName: "house”)\`.  You must install the “SF Symbols” application available at  https://developer.apple.com/design/human-interface-guidelines/sf-symbols/overview/ to see the full list of 1500 available symbols or create your own.

SwiftUI tags will not be visible to UIViews and vice versa.  To get around the limitation you must manually inject the tag as a constructor argument into your UIViews.

UIViewControllerRepresentable, the interoperability protocol for creating custom SwiftUI Views with UIKit, is preventing from receiving focus if it’s nested in a SwiftUI ScrollView.  To get around this you should use SwiftUI TextFields.  When this is not possible (I.e. you need UITabBarItems above the keyboard) you should use a VStack parent instead.
