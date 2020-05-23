---
title: Modern Android Development
date: '2020-05-01T07:24:00-07:00'
---
<img style="float: left; margin:0 2em 1em 0; width: 50%" src="/img/blog/modern.jpg"/> 

"onSaveInstanceState" is called before "onStop()" but not necessarily before "onPause()". It may also never be called (i.e. on back press). 

Only do FramentTransactions inside "Activity.onCreate()", "Activity.onPostResume()" or "FragmentActivity.onResumeFragments()". Otherwise you'll get an exception. You should avoid using FragmentTransactions in "AsyncTask.onPostExecute()" because there is NO lifecycle safety within that scope.  As a lifecycleObserver, a VM can actually implement lifecycle callback methods, avoiding fragmentTransaction errors entirely. Another safe solution is to just put fragment transactions in the activity's LiveData observer.  You should not pass in the backing data structure to an async call. Instead you should use immutable objects and replace the original object instead of modify it. You can use CMD + SHIFT + T to quickly autogenerate a new unit test for the method that your cursor is currently over.



You can use the 3rd party plugin "Save Actions" to optimize imports automatically on save (amongst other things).

To use coroutines you must include the kotlinX gradle dependency. For coroutines you can use either "launch { ... }", which does not return a task, or "async { ... }" which returns a deferred async task.

In Android 7.0 / Nougat JIT was added back to ART for a hybrid JIT/AOT approach. As of android P you will receive compiler warnings if you try and extend private interfaces. SystemAlertWindow on Android allows an app to draw on top of other apps. As of API 26 it was deprecated in favor of TypeApplication.Overlay. Android ID offers a hardware device id similar to an IMEA, with the exception being that it is unique to each app and is changed every time a device is reset.

With Google's new "Timed Publishing" promotions to ALpha/Beta/Production builds will not go live until you press "Go Live".

 "Jazz Hands" is an API for devices that support more than 8 simultaneous finger inputs.

LRU (Less Recent Used) cache is used by default by the Google libraries Glide and Volley.

Timber is now preferred over Log in Kotlin. This is because it auto-ggrabs the class name for the TAG and does not log in production. You just need to ensure that you plant the Timber instance in your Application Singleton. In android studio you can filter out repetitive debugger messages with exclusionary regexes.

Photo by Meriç Dağlı on Unsplash
