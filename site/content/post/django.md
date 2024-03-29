---
title: Django (part 2)
date: '2020-07-01T07:04:31-07:00'
---
![Python](/img/blog/python2.jpg)

This is the second part on Django programming in a two part series.  For context, recently a startup that I work part-time for asked that I transition to back end development.  They were happy to have me help, even after hearing my disclaimer that I had little experience with the framework that they were currently using, Django.  Eager to learn, I accepted the offer and have spent the past month learning the ropes and implementing features here and bug fixes there.  This part will be on the lessons I've learned after diving into the deep end of developing a Django brown-field project.  I also have a [tangentially related post](/post/aar-pt-2-python/) in my old AAR series specifically about python development.  

As a rule of thumb, you should always use Django with virtualenv, or virtual environment.  It's safe and you can install apps in the virtualenv while not affecting the system's environment.  To this end you can either use the virtualenv library that comes pre-packaged with python, or you can pip install virtualenvwrapper.  The benefit of the latter is that it will not add all the virtual environment executables to your project. 

When working with virtual environments you should remain cognizant of all the different conflicts that can occur between the different python sources and python /site-packages libraries.  Some potential conflicts could be your MacOS pre-installed python, your brew installed python, or your virtualenv installed python.

This particular project used a combination of Django templates and embedded React pages.  The Django template language is very similar to the view binding used SwiftUI and androidx.  It allows you to reference python objects from within any number of formats, to include HTML, XML, and CSV.  Variables are indicated by an enclosing double curly brace, i.e. `{{ variable }}`.  

The Django template language also has the concept of tags.  The format for tags is `{% yourTagLogic %}`. You can use tags for basic iteration, selection, and custom functions that you can build yourself.  It's important to note that most template filters return strings, so mathematical comparisons will not work.

You can also use special tags called blocks.  They are formatted the same as a tag, but have the word `block` preceding the enclosed tag logic.

If you plan on a more advanced layout you can use a front-end framework like Angular or React. You’ll need to initiate the client project in the same git repository as the Django project, and then modify the settings.py for the project to include a STATIC_ROOT path that points to the \`/dist/client\` directory and modify the urls.py to import the relevant static files and url patterns that Django needs to match url queries to pages.  This was the approach that the startup had taken.  

It is a widely documented deficiency in python that you cannot import files from sibling packages though.  To get around this limitation of python you need to install your front-end application as a pip dependency of your own project so that you can recursively refer to your project in import statements.  See <https://stackoverflow.com/a/50193944/4520965> for additional details.  Make sure the name referenced in your setup file matches the project directory name exactly.  Appropriately enough you should also always use snake_case in python projects, to include your directory naming convention.  Kebab-case, camelCase and PascalCase will have unpredictable results.

In CSS (and the HTML framework bootstrap) `align` refers to vertical alignment (top, middle, bottom) while `justify` refers to horizontal alignment (left, center, right).  In order for justify-content-center to work in bootstrap it must have nested col div classes, i.e. `<div class="row justify-content-center">`

If you’re using PyCharm you  can configure your virtual environment in the bottom right by setting your run configuration for the DjangoProject. If you’ve already configured the virtual environment in the terminal, just use the “Existing environment” radio button and PyCharm should detect the environment’s presence automatically. Once you have configured the environment for PyCharm you can use the "Run" sub-menu to have PyCharm run the project, execute step-through debugging, analyze stack traces, inspect variables and execute commands.  When evaluating expressions in PyCharm you must call the function, i.e. `list.count() > 1`. When writing expressions in the Django template you must name the function, I.e.   `list.count > 1` 

When working with React \`npm run watch\` can replace typical gulp/grunt watch and auto-build flows.  Delightfully enough, PyCharm has it's own npm tool window, which you can view in PyCharm by choosing "Show npm scripts" in the package.json right-click menu.  You’ll need the tool window in order to execute \`start\` within PyCharm.  If you have an scss file that isn’t being processed, make sure that it’s listed in the app.scss file as an \`@import\`.  In order to debug a React or Angular app in PyCharm you’ll need to add a Javascript Debug configuration in your run configurations.  It should use the same localhost url that your Django project uses.  Additional information can be found at   <https://www.jetbrains.com/help/pycharm/debugging-javascript-in-chrome.html>. If your React or Angular application hangs in PyCharm, try turning off breakpoints and reloading the page, then turning them back on.    

If you want to connect to a database you can do so by opening the "Database" tool window and clicking the + icon.  From there you can add a data source from any number of services.  If you’re trying to connect to a localhost instance of a Postgres database as an example, you would select “Postgres” and then add your database name in the “Database” field.  Additional information can be found at <https://www.jetbrains.com/help/pycharm/connecting-to-a-database.html>. You can view and edit data in a particular table by double clicking the table in the manager view.  Once you’re finished with your changes press CMD + Enter to commit.  To view related data in a particular cell press CMD + Down Arrow.

While working on the project I found that PyCharm curiously unable to step into libraries like it normally does for statically compiled languages like C# or Java.  That's when I found that the standard tool for debugging python applications is PDB.  PDB stands for Python DeBugger. The normal means of using it is to modify your source code by inserting the line `import pdb; pdb.set_trace()` where you would like the debugging process to start.  Once this line is inserted in your code you can run it as you normally would.  Alternatively you can start the python REPL, execute `import pdb` and then execute the command for starting your program.

Common commands for pdb include:

* **l(ist)** - Displays 11 lines around the current line or continue the previous listing.  It’s better to us 
* **ll (long list)** which displays the entire function.
* **s(tep) **- Execute the current line, stop at the first possible occasion.
* **n(ext)** - Continue execution until the next line in the current function is reached or it returns.
* **b(reak)** - Set a breakpoint (depending on the argument provided).
* **r(eturn) **- Continue execution until the current function returns.
* **h(elp)** - Print the list of available commands.  When a command is included as an argument, it displays the man page for that command.
* **!** - Anything following this will be executed in the Python REPL, rather than the pdb REPL.

When debugging you can type the name of a variable that is in scope to view its value.  You can also call the dir() function on instance and check what methods and attributes it has. You can exit pdb just like you would the python REPL, by pressing CTRL+D. If you start pdb from within the python REPL and run the program and hit an unhandled exception you can return the pdb REPL to just before the crash by executing `pdb.pm()` To practice executing all these command yourself, clone the project available at <https://github.com/spiside/pdb-tutorial> and follow the tutorial in the README.

If you’re debugging a library (as I found myself doing) you can compile & debug the dependency by commenting out the original source in the requirements.txt (by pre-pending a `#`) and providing an absolute directory path (i.e.  `file:///home/username/workspace/mypackage#egg=mypackage`).  Note that you will need to execute `pip install -r requirements.txt` every time you make a change to the library.

If you want to get the raw string representation of a value as if you had output it in the terminal, you can use `repr(your_var)` rather than `str(your_var)`.  If you have a library that doesn’t seem to update environment variables after you’ve updated them in the settings.py file, try updating the main project’s settings.py instead.

<span>Photo by <a href="https://unsplash.com/@davidclode?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">David Clode</a> on <a href="/s/photos/python?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>
