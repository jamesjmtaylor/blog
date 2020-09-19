---
title: The Final AAR
date: '2019-12-07T06:04:13-08:00'
---
<img style="float: left; margin:0 0 1em 0; width: 100%" src="/img/blog/different.jpg"> If you haven’t had a chance to read the first entry in the series for context, <a href="/post/after-action-review-aar/">you can do so here</a>.  

For the past couple years I've been doing an AAR (After Action Report) every other month.  This was for several reasons: it helped me synthesize what I had learned over the past few months, it documented what I had personally struggled with, and the material was readily available (I jot down notes everyday).  But having gone back and read through my past AAR blog entries I came to a realization.  The posts aren't terribly fun or compelling to read.  For one, they're repetitive.   They start with the same vanilla reference back to the original entry and end with a list of dry bullet points.  They can also be somewhat esoteric, given the complete lack of context for each bullet point.  

So, going forward I plan to do more entries like last month's SwiftUI post.  While still a bit on the technical side, it at least provided some context to each point and seemed to flow more naturally.  There's also the added bonus that by reconstructing the circumstances that drove me to catalog those bullet points I'm more likely to retain the lessons learned myself. 

At Nautilus I've been doing a lot of work with Swift recently.  This has been because the iOS app has been swamped with bug fixes and technical debt stories as result of the rush to finish the 1.3.0 JRNY app release. I had a bit of a culture shock in switching back to iOS from Android work. I found that backgrounding the app and bringing it back up does NOT call viewWillAppear/Disappear like it does on Android.  You need to explicitly close and open the windows in the AppDelegate methods to trigger the method. I was also reminded that one shouldn't manually modify the info.plist file source code.  This is because it can be updated by the IDE and Apple at will.  It also turns out that each tab in a TabBar instantiates a new ViewController, even if they all point to the same ViewController in the storyboard.  You must introspect on the tabBar's selected tab to know where you truly are if you re-use ViewController layouts. You can programmatically drive user navigation to different tabs by using a router pattern.  This is particularly useful for deep links in iOS.  You have to register the deep link url schema in your AppDelegate, but once this is done you can use it to tell your router where to take the user.  On the topic of routers, creating unwind segues between two different storyboards is possible as long as they are directly related.  Trying to unwind from one storyboard to another when there are one or more intermediary storyboards between them will not work.  To make routers a little less verbose you can use typealiases to substitute a long chain of objects for a much more concise term (i.e. FirstChild for UIKit.UINavigationController.children.first).

JRNY is an internet-driven app.  Part of my work in the app was interacting with our AWS backend.  To that end I found that the Swift URL library allows you to instantiate a URL not only from a raw string, but also by creating a URLComponents object, assigning it a scheme, host, path, and query items, and then returning components.url.  When implementing RESTful encoding & decoding I found that the swift Codable protocol does not actually support multi-part form data payloads.  For that you have to either encode it by hand or use an external library such as AlamoFire.  As an interesting sidenote Swift has it's own server framework called Kitura for the aspiring full-stack swift developer. One of the popular Swift asynchronous libraries used in support of networking is RxSwift.  In RxSwift you must explicitly access the main thread for Timers in observable callbacks.  This is because observable callbacks don't have access to the main thread in Rx's scope and will not fire. You can use \[unowned self] in an RxSwift completion blocks to avoid calling self when it's nil.  As a best practice you should generally capture viewController context in asynchronous call closures with the annotation \[weak self].  This will help prevent memory leaks due to long-running asynchronous processes. As of Swift 4.2 you should NOT try to implement your own hasher. This is because by conforming to the Swift 4.2 Hashable protocol you get default hash function implementations for free.  This free implementation will generally be superior & faster to anything you might develop ad-hoc.

The JRNY app has a fair amount of legacy ObjectiveC code.  ObjC can do just about everything Swift can. This includes Generics (supported by the LLVM), protocol oriented programming (difficult but not impossible), and higher order functions (through ObjC blocks). One thing ObjC cannot do is call Swift code.  The opposite is not true though; Swift can call ObjC, and through it, C code.  ObjC typeDefs are anonymous objects that you can pass as arguments.  An excellent example is "Logging.setCallback(...)" which expects an anonymous callback object conforming to the typeDef.  In iOS you can use subArrays in a function by having the argument as "Func myFunc<T:Seguence>(things:T) -> String where T.Iterator.Element: String".
 C types are static (enforced at compile time) but ObjC classes are dynamic (enforced at run-time). Structs in Swift are value types and will always be pass by value, even into functions.  This means a new copy of the object is created when a struct is passed as a parameter.  Arrays, dictionaries, and sets are also all value types.  Classes that are children of structs CAN be mutated.  In swift you can curry parameters for 1st order functions by just passing them with their parameters blank, i.e. "setUserAndGreet(greetingOfTheDay(user: ))"

While executing TDD (Test Driven Development) for my iOS work I re-learned a few XCode gotchas. After changing target names in the XCode project you should manually delete the derived data. You also need to manually re-point your unit tests targeting by changing it in "Build Settings" > "Bundle Loader & Test Hosts".  Unit tests can only be run against one build Target.  When running the unit tests in a project with Firebase cocoapods you should include the \`Firebase Core\` pod in the Unit Tests Target to avoid compilation errors when running the tests.  When running unit tests on a class with CoreData you must implement your own CoreData mock.  Otherwise the cast from AppDelegate to the Unit Test AppDelegate will fail.  Finally, when manually testing your app in an iOS simulator you cannot turn on airplane mode because iOS processes are really just macOS processes and MacOS does not have the ability to control internet access on an app-by-app basis.  You CAN introspect on UserDefaults  in the console however while debugging your app by inspecting the "UserDefaults.standard.dictionaryRepresentation"

At the end of a sprint I've found it helpful to create pre-recorded videos of newly implemented functionality, both as git/Jira documentation of the completed tickets as well as for the benefit of demos to the stakeholders.  To this end you can view where you are tapping by turning on assistive touch and creating a custom tap gestures.   When you finally launch a new version of your app you can erase your ratings and reviews, but you don't have to.

<a href="https://sebprovencher.com/2019/09/12/and-now-for-something-completely-different/">Photo credit to Seb Provencher</a>