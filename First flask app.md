#Making our first flask app
Today we are going create a database powered flask app. This code was inspired by
[this Python Anywhere blog post](https://blog.pythonanywhere.com/121/) and if you get stuck it might be a great resource to help you out. 

##Step 1 - Setting up version control

Version control allows us to work together better and rollback any changes we might have made on accident.

Open up console tab and use the github username and password you set up earlier in the class.

Let's first make sure we are in the mysite folder.

```
cd mysite
```

```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

From there you let's start git so that we can have our first repo.

```
git init
```

We will need to tell git what to files to keep private by creating what's known as a git ignore

To create this file, type the following command:

```
cat > .gitignore
```

You will be taken to a blank page where you should type the following in:

```
*.pyc
__pycache__
```

Make sure you press control-d when finished.

Let's do a git status to make sure everything is working well.

```
git status
```

Now we can add in the files we created.

```
git add .gitignore flask_app.py
```

Now we can commit our changes:

```
git commit -m"First commit of a really simple web app, with a .gitignore file"
```

Lets run another git status

```
git status
```

##Step 2 - Creating a New Page for Our Site

Currently our page is at http://yourusername.pythonanywhere.com/ and if we go to http://yourusername.pythonanywhere.com/about nothing will happen. But if we go back to our source code.

Now if we add this code to our existing app we can create an about page for our site.

```python
@app.route('/about')
def about():
    return 'This is my about page'
```

Make sure your press the save and reload buttons and now go to http://yourusername.pythonanywhere.com/about and you will now see the about page you just created.

##Step 3 - Undoing Mistakes

What we if we didn't want to add that about page. We can use git to roll back those changes.

Let's do a git status to see where we are.

```
git status
```

Since we haven't checked in any changes yet. We can just check out our previous version of the file we were working on.

```
git checkout -- flask_app.py
```

Let's do a another git status to see where we are.

```
git status
```

Now we can double check to see if we our changes have been removed.

##Step 4 - Creating our Template

Templates allow us to keep our HTML separate from the logic of our app to make things more workable.

Let's go into the console and make a new directory and a file.

```
mkdir templates
cd templates
touch main_page.html
```

Now we can go to our files and edit the main_page.html file and edit it to have the following content.

```
<html>
    <head>
        <title>Scratchpad</title>
    </head>

    <body>

        <div>
            This is the first comment.
        </div>

        <div>
            This is the the second comment.  It's no more interesting
            than the first.
        </div>

        <div>
            This is the third comment.  It's actually quite exciting!
        </div>

        <div>
            <form action="." method="POST">
                <textarea name="contents" placeholder="Enter a comment"></textarea>
                <input type="submit" value="Post comment">
            </form>
        </div>

    </body>
</html>
```

Now edit flask_app.py to look like this

```python
from flask import Flask, render_template

app = Flask(__name__)
app.config["DEBUG"] = True

@app.route("/")
def index():
    return render_template("main_page.html")
```

We can now commit our changes back into git.

```
cd ..
git add templates/
git add flask_app.py
git commit -m"now with templates"
git status
```

##Step 5 - Making our site look more modern

We can use bootstrap to make our site look a bit more modern. Let's back into our template and make the following edits.

```
<html>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" integrity="sha512-dTfge/zgoMYpP7QbHy4gWMEGsbsdZeCXz7irItjcC3sPUFtf0kuFbDz/ixG7ArTxmDjLXDmezHubeNikyKGVyQ==" crossorigin="anonymous">

        <title>My scratchboard page</title>
    </head>

    <body>
        <nav class="navbar navbar-inverse">
          <div class="container">
            <div class="navbar-header">
              <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
              </button>
              <a class="navbar-brand" href="#">My scratchpad</a>
            </div>
          </div>
        </nav>

        <div class="container">

            <div class="row">
                This is the first dummy comment.
            </div>

            <div class="row">
                This is the the second dummy comment.  It's no more interesting
                than the first.
            </div>

            <div class="row">
                This is the third dummy comment.  It's actually quite exciting!
            </div>

            <div class="row">
                <form action="." method="POST">
                    <textarea class="form-control" name="contents" placeholder="Enter a comment"></textarea>
                    <input type="submit" value="Post comment">
                </form>
            </div>

        </div>

    </body>
</html>
```

Let's check our code in for this step back in the console.

```
git add templates/main_page.html
git commit -m"Made the site prettier."
git status
```

##Step 6 - Changing the app to GET and POST data

Right now, if we attempted to make a comment on the site it would give us errors. If we wanted to do so we'll need to change the template to allow for this. Let's go back to the code for app and change it to the following:

```python
from flask import Flask, redirect, render_template, request, url_for

app = Flask(__name__)
comments = []
app.config["DEBUG"] = True

@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "GET":
        return render_template("main_page.html", comments=comments)

    comments.append(request.form["contents"])
    return redirect(url_for('index'))
```

Let's go back to the console and check our code in

```
git add flask_app.py
git commit -m"Added git post."
git status
```

##Step 7 - Let's change the template to be more flexible.

We can change the template to make be more robust by deleting the comments we had in there and replacing them with a loop that iterates over them to put a div around each comment.

```
<html>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" integrity="sha512-dTfge/zgoMYpP7QbHy4gWMEGsbsdZeCXz7irItjcC3sPUFtf0kuFbDz/ixG7ArTxmDjLXDmezHubeNikyKGVyQ==" crossorigin="anonymous">

        <title>My scratchboard page</title>
    </head>

    <body>
        <nav class="navbar navbar-inverse">
          <div class="container">
            <div class="navbar-header">
              <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
              </button>
              <a class="navbar-brand" href="#">My scratchpad</a>
            </div>
          </div>
        </nav>

        <div class="container">

            {% for comment in comments %}
                <div class="row">
                    {{ comment }}
                </div>
            {% endfor %}

            <div class="row">
                <form action="." method="POST">
                    <textarea class="form-control" name="contents" placeholder="Enter a comment"></textarea>
                    <input type="submit" value="Post comment">
                </form>
            </div>

        </div>

    </body>
</html>
```

Going back to the console let's check our code back in.

```
git add templates/main_page.html
git commit -m"Made the template more robust."
git status
```

##Step 8 - Adding a database

Go to the databases tab of python anywhere and make sure you are in MySQL and create a new password to initialize the database. It may take a minute to get set up and from there go to where it says create new database, name your database comments, and press the green button create.

##Step 9 - Connecting the database into our app
Now we can add our database connection to our app by using SQLAlchemy to connect back in. We will need to edit the following code to our own credentials to make this work.

```python
from flask import Flask, redirect, render_template, request, url_for
from flask.ext.sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config["DEBUG"] = True
SQLALCHEMY_DATABASE_URI = "mysql+mysqlconnector://{username}:{password}@{hostname}/{databasename}".format(
    username="the username from the 'Databases' tab",password="the password you set on the 'Databases' tab",
    hostname="the database host address from the 'Databases' tab",
    databasename="the database name you chose, probably yourusername$comments"
)
app.config["SQLALCHEMY_DATABASE_URI"] = SQLALCHEMY_DATABASE_URI
app.config["SQLALCHEMY_POOL_RECYCLE"] = 299
db = SQLAlchemy(app)
class Comment(db.Model):

    __tablename__ = "comments"

    id = db.Column(db.Integer, primary_key=True)
    content = db.Column(db.String(4096))
comments = []

@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "GET":
        return render_template("main_page.html", comments=comments)

    comments.append(request.form["contents"])
    return redirect(url_for('index'))
)
```

Now we can go into the console and set up our database

```
ipython
from flask_app import db
db.create_all()
```

If you wanted to go see the sql you just created in action you can go back to the database tab. You can click on the console listed where it says "Your Databases" under comments. You will get a MySQL command line where you can run the following queries.

```
show tables;
```
```
describe comments;
```

Now let's go back to our console to commit our code back in.

```
git add flask_app.py
git commit -m"Connected the app to the database."
git status
```

##Step 10 - Let's change our template

Let's go into our template and change where it says {{ comment }} to {{ comment.content }}

Going back to the console let's check our code back in.
git add templates/main_page.html
git commit -m"Made the template database ready."
git status

##Step 11 - Let's change the app so that we query the database for comments.

Let's change the code to this:
```python
from flask import Flask, redirect, render_template, request, url_for
from flask.ext.sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config["DEBUG"] = True
SQLALCHEMY_DATABASE_URI = "mysql+mysqlconnector://{username}:{password}@{hostname}/{databasename}".format(
      username="the username from the 'Databases' tab",
      password="the password you set on the 'Databases' tab",
      hostname="the database host address from the 'Databases' tab",
      databasename="the database name you chose, probably yourusername$comments",
)
app.config["SQLALCHEMY_DATABASE_URI"] = SQLALCHEMY_DATABASE_URI
app.config["SQLALCHEMY_POOL_RECYCLE"] = 299
db = SQLAlchemy(app)
class Comment(db.Model):

    __tablename__ = "comments"

    id = db.Column(db.Integer, primary_key=True)
    content = db.Column(db.String(4096))

@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "GET":
        return render_template("main_page.html", comments=Comment.query.all())

    comment = Comment(content=request.form["contents"])
    db.session.add(comment)
    db.session.commit()
    return redirect(url_for('index'))
```

Now let's go back to our console to commit our code back in.

```
git add flask_app.py
git commit -m"Used the comments from the database."
git status
```

##Step 12 - Query your database
Now that your app is finished you can add a comment to the site. Let's now go to the database console to see if we can see it in the database.

```
select * from comments;
```

That's so exciting! We made our first app.
