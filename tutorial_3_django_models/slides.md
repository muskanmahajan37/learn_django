Tutorial: Create Django Models
===============================
[Demonstration time: 9 mins 25 secs (0.785 ~ 79%) | Total time: 12 mins]

Slide 1 [00:08 | 00:08]
-------------
Title Slide: ** Creating Django Models **

Slide 2 [00:12 | 00:20]
--------------

**Learning Objectives**

In this tutorial, we will learn to;
  - Create a django model
  - Perform Database migration
  - Use Admin app

Slide 3 [00:11 | 00:31]
---------------

**System Requirements**
  - Ubuntu 16.10
  - Python 3.5 or higher version
  - python3.4-venv
  
Slide 4 [00:11 | 00:42]
---------------

**Pre-requisites**

In order to follow this tutorial, you need to know;
  - how to create a django project
  - If not, see the relevant django tutorial on http://spoken-tutorial.org


Slide 4: What is a Django Model [00:30 | 01:12]
--------------------------------------

  - Every Django app has one or more models
  - A Django model is a pythonic representation of a Database Table
    - A Model Class represents a database table
    - A Model attribute represents a table field (each column in the table) 
    - Each model instance is a table record (each row in the table)
  - All django models are written in a models.py
    - models.py represents a Database

Slide 5: Creating Django Models for Blog app [00:12 | 01:24]
-----------------------------------------------

  - We will create two tables 'blog' and 'article'
  - The blog model will have 3 fields:
    - name
    - created_on
  - the 'article' model will have 5 fields:
    - blog
    - created_on
    - title
    - body
    - draft
    
  - **(Not for narration) Note: Explain this in detail during demonstration**

Demonstration [04:00 | 05:24]
----------------

  - open */blog/models.py* in editor
  - Type the following code

        from django.db import models

        class Blog(models.Model):
            name = models.CharField(max_length=120)
            created_on = models.DateTimeField('date created')


        class Article(models.Model):
            blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
            created_on = models.DateTimeField('date created')
            title = models.CharField(max_length=120)
            body = models.TextField()
            draft = models.BooleanField(default=False)

   - **(Not for narration) Note:  For this demonstration there should explanation about each field **

Demonstration [01:00| 06:24]
--------------------
- Run python manage.py --help
  - Show the help text for *makemigrations* command
  - Show the help text for *migrate* command

- Go to the directory containing *manage.py* file
  - Make sure that the environment is active
  - Run the command to create database migration files
    - *python manage.py makemigrations*
  - Run the command to apply these migrations
    - *python manage.py migrate*
  - Output:
    - *Show command line output for makemigrations and migrate*
    - Observe that a *db.sqlite3* has been created.
    - Check the /blog/migrations/ directory and you will observe that a new file has been created.
  - We will be revisiting *migrations* in more detail later

Slide 5: What is Django Admin App [00:07| 06:31]
-------------------------------------------

  - What is the Django Admin App
    - The django admin app is a feature that provides an interface to Create, Read, Update and Delete model instances

Demonstration: Create a superuser [00:55| 07:26]
----------------------------------------------
  - Create a superuser to login to the admin interface
    - Run *python manage.py createsuperuser*
      - Provide a username
      - Provide an email address
      - Provide a password and confirm it by entering again
  - Output: *Superuser created successfully.*


Demonstration: Show the Admin interface [00:45| 08:11]
---------------------------------
  - Run the django server with
    - *python manage.py runserver*
  - Open the web browser and go to http://127.0.0.1:8000/admin/
  - You will see the login screen
      - [Add Screenshot]
  - Enter the superuser credentials as created previously
  - You can see the interface
    - [Add Screenshot]
    
Demonstration: Make the Blog App modifiable through Admin [01:10 | 09:21]
-------------------------------------------
  - In order to display the Blog app and it's components in the admin interface, change the */blog/admin.py* file;

    # /blog/admin.py
    from django.contrib import admin

    from .models import Blog, Article

    admin.site.register(Blog)
    admin.site.register(Article)
  - Save
  - Login to the Admin interface

Demonstration: Add a Blog database entry [00:45| 10:06]
-----------------------
  - Click on 'Blog'
  - Click on 'Add Blog'
  - Fill the form for new blog
    - name
    - creator
    - create_date
  - Click on Save
  
  - You will see your new blog in the list
    - You can click on the blog name to view it's details
  - Click on Home (in breadcrumb section)
  
Demonstration: Add an Article database entry [00:50| 10:56]
--------------------------------
  - Click on 'Article'
  - Click on 'Add Article'
  - Fill the form for the new Article
    - blog: Select 'My New Blog' from list
    - title
    - create_date
    - author: Select super-username form the list
    - body: Add Article text
  - Click on Save
  - You will see your new article in the list
    - You can click on the article name to view it's details
    

So this is a web application framework.
Slide 6 [00:17 | 11:13]
----------------

**What is a web application**

![Block diagram of Web application](https://raw.githubusercontent.com/FOSSEE/learn_django/master/tutorial_1_intro/webapp_diag.png 'Web Application Block diagram')
**Can be used for narration for this slide**	
  - An application stored on a remote computer i.e. server
  - A server can be accessed by a user through a web browser
  - The browser communicates with the server by sending a 'request'
  - The web application carries out actions as per the request
  - It has a 'database' to store and manipulate data
  - It sends a 'response' to the user
  - The user's browser then displays this response suitably formatted.

Slide 7 [00:05 | 11:18]
------------------

**What is a web Framework**
  - Easy to develop web apps
  - Provides
    - Interface to Database
    - Authentication (Login system)
    - Templating engine (HTML rendering)
    - Forms

 
 
 *** With this we come to the end of the tutorial***
 ----------------------------------------------------
 *** Add concluding slides and assignment***[00:42 | 12:00  ]
 -------------------------------------------
