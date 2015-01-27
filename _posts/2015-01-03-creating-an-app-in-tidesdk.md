---
title:  "Creating an app in TideSDK: Six steps to go"
date:   2015-01-05 17:08:00
categories: TideSDK
permalink: /TideSDK/creating-an-app-in-tidesdk.html
---

<p>
  Here you will learn how to create a basic app in <a href="http://www.tidesdk.org">TideSDK</a>. TideSDK is an open source frame work for creating desktop applications using html, css, javascript as front-end and any language like python or php as back-end. You can find the setup and installation instructions <a href="http://tidesdk.multipart.net/docs/user-dev/generated/#!/guide/getting_started-section-3">here.</a>
</p>
<p>First you need to install TideSDK. Now open the TideSDK developer and follow the steps:</p>
<ul>

  <li>Click on 'New Project'.</li>

  <li>
    Fill in the required information. Use the data given below:
    <pre>
      Project type        : Desktop
      Name                : DemoApp
      App Id              : com.myapps.demoapp
      Directory           : (Use the path of any directory where you wish to create the app.)
      Company/Personal URL: (Use your site url.)
    </pre>
  </li>

  <li>
    Click 'Create Project'
    <p>As you click the create button, you can see the information of your application in the same window with some additional information.</p>
    <p>Now browse to the directory in which you created the app. You will see a folder named DemoApp there. You can see some folders and files in it. You will find detailes about them <a href="http://tidesdk.multipart.net/docs/user-dev/generated/#!/guide/getting_started-section-5">here</a>. Here is the directory structure you will get:
      <pre>
  |-- CHANGELOG.txt  
  |-- LICENSE.txt
  |-- dist
  |-- Resources
  |   |-- index.html
  |-- manifest
  |-- tiapp.xml
      </pre>
    </p><br>
  </li>

  <li>
      Now you have to create some files in the resources directory.<br>
      <p>
      First copy the <i>default_app_logo.png</i> from \path\to\TideSDK Developer\Resources\default_app_logo.png into the resources directory.
      </p>
    <strong>app.js</strong>
    <div class="wrapper" style="text-align:justify;">
      {% highlight javascript %}

      var menu = Ti.UI.createMenu(),
      fileItem = Ti.UI.createMenuItem('File'),
      exitItem = fileItem.addItem('Exit', function() {
        if (confirm('Are you sure you want to quit?')) {
          Ti.App.exit();
          }
          });
      menu.appendItem(fileItem);
      Ti.UI.setMenu(menu);      

      {% endhighlight %}
    </div>

    <strong>index.html</strong>
    <div class="wrapper" style="text-align:justify;">
      {% highlight html %}

    <!DOCTYPE html>
    <html>
      <head>
        <title>My demo app</title>
      </head>
      <body>
        <div>
          Welcome to the demo app created with TideSDK.
        </div>
        <script type="text/javascript" src="app.js"></script> 
      </body>
    </html>

      {% endhighlight %}
    </div>
  </li>

  <li>
    Now in the developer, switch to the Test & Package tab of the window. Click 'Launch App'.<br>
    You can see the live demo of your application.
  </li>

  <li>Now click 'Package with Runtime'.<br>
  You can now see a folder named packages in your app's directory. Browse to packages/win32/bundle/DemoApp/ and there you can see your application file along with some other necessary files.
  </li>
</ul>

<p>
  You have a desktop app that you can now run locally. For packaging the app for distribution, you will need some extra tools. We will see that in next post. Till then play with your app and do some useful stuff.
</p>