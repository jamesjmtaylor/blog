---
title: Django
date: '2020-06-27T07:04:31-07:00'
---
<img style="float: left; margin:0 2em 1em 0; width: 50%" src="/img/blog/python2.jpg"/> Recently a startup that I work part-time for asked that I transition to back end development.  They were happy to have me help, even after hearing my disclaimer that I had little experience with the framework that they were currently using, Django.  Eager to learn, I accepted the offer and have spent the past month learning the ropes and implementing a few features and bug fixes here and there.   This post will be two parts.  The first will cover my philosophy on how I charge for this kind of work.  The second will be on the lessons I've learned after diving into the deep end of developing in a django brown-field project.

The Django template language is very similar to the view binding used SwiftUI and AndroidX.  It allows you to reference python objects from within any number of formats, to include HTML, XML, and CSV.  Variables are indicated by an enclosing double curly brace, i.e. \`{{ variable }}\`.  

The Django template language also has the concept of tags.  The format for tags is \`{% yourTagLogic %}\`. You can use tags for basic iteration, selection, and custom functions that you can build yourself.  Remember that most template filters return strings, so mathematical comparisons will not work.

You can use special tags called blocks.  They are formatted the same as a tag, but have the word \`block\` preceding the enclosed tag logic.

If you plan on a more advanced layout you can use a front-end framework like Angular or React. You’ll need to initiate the client project in the same git repository as the django project, and then modify the settings.py for the project to include a STATIC_ROOT path that points to the \`/dist/client\` directory and modify the urls.py to import the relevant static files and url patterns that django needs to match url queries to pages.

It is a widely documented deficiency in python that you cannot import files from sibling packages.  To get around this limitation of python you need to install your project as a pip dependency of your own project so that you can recursively refer to your project in import statements.  See https://stackoverflow.com/a/50193944/4520965 for additional details.  Make sure the name referenced in your setup file matches the project directory name exactly.

Always use snake case in python projects, to include directories.  Kebab-case, camelCase and PascalCase will have unpredictable results.

In CSS (and bootstrap) align refers to vertical alignment (top, middle, bottom) while justify refers to horizontal alignment (left, center, right).  In order for justify-content-center to work in bootstrap it must have nested col div classes, i.e. <div class="row justify-content-center">

If you’re using PyCharm you  can configure your virtual environment in the bottom right by setting your run configuration for the DjangoProject. If you’ve already configured the virtual environment in the terminal, just sealed the “Existing environment” radio button and PyCharm should detect the environment’s presence automatically. Once you have configured the environment for PyCharm you can use the Run sub-menu to have PyCharm run the project, execute step-through debugging, analyze stack traces, inspect variables and execute commands.  

\`npm run watch\` can replace typical gulp/grunt watch and auto-build flows.  If you have an scss file that isn’t being processed, make sure that it’s listed in the app.scss file as an \`@import\`. 

When evaluating expressions in PyCharm you must call the function, i.e. \`list.count() > 1\`. When writing expressions in the django template you must name the function, I.e.   \`list.count > 1\`

You can view the npm tool window in PyCharm by choosing Show npm scripts in package.json right-click menu.  You’ll need the tool window in order to execute \`start\` within PyCharm.

In order to debug a React or Angular app in PyCharm you’ll need to add a Javascript Debug configuration in your run configurations.  It should use the same localhost url that your Django project uses.  Additional information can be found at   https://www.jetbrains.com/help/pycharm/debugging-javascript-in-chrome.html

If your React or Angular application hangs, try turning off breakpoints and reloading the page, then turning them back on.  

If you want to connect to a database you can do so by opening the database tool window and clicking the + icon.  From there you can add a data source from any number of services.  If you’re trying to connect to a localhost instance of a Postgres database as an example, you would select “Postgres” and then add your database name in the “Database” field.  Additional information can be found at https://www.jetbrains.com/help/pycharm/connecting-to-a-database.html. 

You can view and edit data in a particular table by double clicking the table in the manager view.  Once you’re finished with your changes press CMD + Enter to commit.  To view related data in a particular cell press CMD + Down Arrow.

<span>Photo by <a href="https://unsplash.com/@davidclode?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">David Clode</a> on <a href="/s/photos/python?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>
