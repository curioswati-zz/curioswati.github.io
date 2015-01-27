---
title:  "How to use a python script in TideSDK: with an example"
date:   2015-01-17 18:08:00
categories: TideSDK python
permalink: /TideSDK/python/how-to-use-a-python-script-in-tidesdk.html
---
<p>
	Today, we are going to see an example for how to use python as back-end in <a href="http://www.tidesdk.org">TideSDK</a>. As we have already learnt to create a basic application in TideSDK in a <a href="http://swati-jaiswal.github.io/TideSDK/creating-an-app-in-tidesdk.html">previous</a> post. We will be using the same application to continue here.
</p>

<p>
	First we need to create an app</a>. As soon as we create it, we have a directory called resources, in the directory where we created the app.
	In that resources directory, we need following files, some of them will already be there:
	<pre>
/Resources
  -/css
    -bootstrap.css
    -app.css
  -default_app_logo.png
  -app.js
  -index.html
  -login.py			
		</pre>
</p>

<p>Now you need to modify the <strong>index.html</strong></p>
	{% highlight html %}

<!DOCTYPE html>
<html>
<head>
  <title>Login-page</title>
  <link rel="stylesheet" type="text/css" href="css/bootstrap.css">
  <link rel="stylesheet" type="text/css" href="css/app.css">
</head>
<body>
  <div class="container credentials">
    <div class="row">
      <label Username: label>
      <input type="text" id="username" value="swati">
    </div>
    <div class="row">
      <label Password: label>
      <input type="password" id="password" value="">
    </div>
    <br>
    <div class="btn btn-success btn-mini" align="center" onclick="Login()">Login</div>
  </div>
  <script type="text/python">>
    import login
    window.log_in = login.main
  </script>

  <script type="text/javascript" src="app.js"></script> 

  <script type="text/javascript">
    var Login = function () {

      username = document.getElementById("username").value;
      password = document.getElementById("password").value;

      logged = window.log_in(username, password)
      console.log(logged)
      if (logged) {
        window.alert("Welcome!");
      }
    };
  </script>
</body>
</html>

	{% endhighlight %}
</p>

<p>
    In the index file, we are having two input boxes for username and password. And one login button. When login is clicked, the Login function is called from the javascript.
    {% highlight javascript %}
    <script type="text/javascript">
  var Login = function () {
  	username = document.getElementById("username").value;
    password = document.getElementById("password").value;
    logged = window.log_in(username, password)
    if (logged) {
    	window.alert("Welcome!");
        location.href="index.html"
        }
        };
</script>
    {% endhighlight %}
</p>

<p>
Here, we have called another function log_in <code>logged = window.log_in(username, password)</code>
This is defined in the python script:
{% highlight javascript %}

	<script type="text/python">
import login
window.log_in = login.main
</script>

{% endhighlight %}
</p>

<p><br>
In the python script part, we have used <code>import login</code>. This is the way to include, a python script. The documentation stated way of using Titanium.include() does not work now, you can see the <a href="https://github.com/TideSDK/TideSDK/blob/1.3.1-beta/CHANGES">1.3.1 beta release notes</a>.<br>
For using python script's functions or classes, we need to declare them in the window object, as the <a href="http://tidesdk.multipart.net/docs/user-dev/generated/#!guide/using_python">Documentation</a> says,
</p>
<blockquote style="text-align:justify;">
	Any class, function or variable that you want to pass from Python back to Javascript must be declared in the window object.
</blockquote>

<p>The following line does this for us, <code>window.log_in = login.main</code></p>
<p>Here, main function from login.py is declared as window.log_in, that can be used anywhere in javascript.</p>

<p>
    For using python in Tide, we have <a href="http://tidesdk.multipart.net/docs/user-dev/generated/#!guide/using_python">three ways</a>.<br>
    We are using the external script. so now you need to create a file named <strong>login.py</strong>
{% highlight python %}
#login.py
def main(user, password):
'''
Module for login.
'''
if user == "swati" and password == "hello":
return True;
else:
return False;
{% endhighlight %}
</p>
<p>
	And we are done. Now the javascript can call our python function, and if login is correct according to script, we are prompted with an alert box.<code>window.alert("Welcome!");</code>
</p>