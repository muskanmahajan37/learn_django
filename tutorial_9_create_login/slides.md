Slide 1 [00:08 | 00:08]
------------
Title Slide
**Django Authentication**

Slide 2 [00:12 | 00:20]
--------------

**Learning Objectives**

In this tutorial, we will learn to;
  - Create a new form
  - Create a new view to handle form submission

Slide 3 [00:11 | 00:31]
---------------

**System Requirements**
  - Ubuntu 16.10
  - Python 3.5 or higher version
  - python3.4-venv

Slide 4:
----------------

**Introduction**

- Let us use our knowledge of forms and templates to create a login system for our blog application
- We will use Django's inbuilt authentication system for this.

Slide 5:
----------------

**Modify the urls.py**

- Modify the urls.py file located in the ```myproject``` folder

    # /myproject/urls.py
    from django.conf.urls import include, url
    from django.contrib import admin
    from blog import views
    from django.contrib.auth import views as auth_views # Add this import


    urlpatterns = [
        url(r'^admin/', admin.site.urls),
        url(r'^blogs/$', include('blogs.urls')),
        url(r'^login/$', auth_views.login, {'template_name': 'login.html'}), # Add this line
    ]

Slide 6:
------------------

**Create a new template**

Create a template login.html at /blog/templates/blog/login.html to look like below

- Create a template ```login.html``` at ```/blog/templates/blog/login.html``` to look like below

        <html>
        <body>
            <form method="post" action="{% url 'django.contrib.auth.views.login' %}">
                {% csrf_token %}
                <p class="bs-component">
                    <table>
                        <tr>
                            <td>{{ form.username.label_tag }}</td>
                            <td>{{ form.username }}</td>
                        </tr>
                        <tr>
                            <td>{{ form.password.label_tag }}</td>
                            <td>{{ form.password }}</td>
                        </tr>
                    </table>
                </p>
                <p class="bs-component">
                    <center>
                        <input class="btn btn-success btn-sm" type="submit" value="login" />
                    </center>
                </p>
                <input type="hidden" name="next" value="{{ next }}" />
            </form>
        </body>
        </html>
