Slide 1 [00:08 | 00:08]
------------
Title Slide
**Creating Forms to edit the Blog**

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

**What is a Form?**

- In HTML, a form is a collection of elements inside <form>...</form>
- Allow a visitor to do things like;
    - enter text
    - select options
    - manipulate objects or controls, and so on,
- Send information back to the server

Slide 5
----------------

**Django Forms**

- Django provides inbuilt libraries to help you build forms easily


Slide 6
-----------------

**What is POST Request**

- A form is what the user sees when the application is storing new *or* updating/deleting existing data on a server,
- In the backend this storage, updation or deletion is done using a POST request
- A POST request method requests that a web server accept the data enclosed in the body of the request message

Slide 7
----------------

**Creating a New Form for Blogs**

- Create a forms.py in the ```blog``` and add the code

      class BlogForm(ModelForm):
          class Meta:
              model = Blog
              fields = ['name', 'created_on']
        
      class ArticleForm(ModelForm):
          class Meta:
              model = Article
              fields = ['created_on', 'title', 'body', 'draft']

Slide 8
----------------

**Adding a new view to handle the form**

- Let's add a new view to add a new blog or edit an existing blog
- To do this add the following code to the ```views.py``` in the ```blog```directory

      from django.http import HttpResponse, HttpResponseNotFound
      from .forms import BlogForm, ArticleForm

      def edit_blog(request, blog_id):
      """
      This view adds a new or edits an existing blog
      """
          if blog_id:
              blog = Blog.objects.get(id=blog_id)
          else:
              blog = Blog()
          if request.method == 'GET':
              blog_form = BlogForm(instance=blog)
              context = {'form': blog_form, 'blog': blog}
              return render(request, 'blog/blog_form.html', context)
          elif request.method == 'POST':
              form = BlogForm(request.POST, instance=blog)
              if form.is_valid():
                  form.save()
                  return redirect('index')
              else:
                  return redirect('edit_blog')
                  
      def edit_article(request, blog_id, article_id):
      """
      This view adds a new or edits an existing article
      """
          blog = Blog.objects.get(id=blog_id)
          if article_id:
              article = Article.objects.get(id=article_id)
          else:
              article = Article(blog=blog)
          if request.method == 'GET':
              article_form = ArticleForm(instance=article)
              context = {'form': article_form, 'article': article}
              return render(request, 'blog/article_form.html', context)
          elif request.method == 'POST':
              if article == None:
                  form = ArticleForm(request.POST, instance=article)
              else:
                  form = ArticleForm(instance=article)
              if form.is_valid():
                  form.save()
                  return redirect('index')
              else:
                  return redirect('edit_article')

Slide 8
-----------------

**Creating a Template for the view**

- Create a template ```edit_blog.html``` at ```/blog/templates/blog/edit_blog.html``` to look like below

        <html>
        <body>
        {% if form %}
            <form action="/blog/edit_blogs/{{ blog.id }}" method="post">
                {% csrf_token %}
                    {{ form }}
                <input type="submit" value="Submit" />
            </form>
        {% endif %}
        </body>
        </html>
        
- Create a template ```edit_article.html``` at ```/blog/templates/blog/edit_article.html``` to look like below

        <html>
        <body>
        {% if form %}
            <form action="/blog/edit_articles/{{ blog.id }}/{{ article.id}}" method="post">
                {% csrf_token %}
                    {{ form }}
                <input type="submit" value="Submit" />
            </form>
        {% endif %}
        </body>
        </html>

Slide 9
-----------------

**Add URLs for editing blogs and articles**

Modify the urls located in the blogs folder

    # /blogs/urls.py
    from django.conf.urls import patterns, url
    from blog import urls

    urlpatterns = [
        url(r'^(?P<username>\w+)/list/$', views.get_blogs, name='blogs'),
        url(r'^/edit_blogs/(?P<blog_id>\d+)/$', views.edit_blogs, name='edit_blogs'),
        url(r'^/edit_articles/(?P<blog_id>\d+)/(?P<article_id>\d+)$', views.edit_articles, name='edit_articles')
    ]

Slide 10:
-----------------

**Edit Templates to add new URL redirect**

- Edit the ```blogs.html``` as follows;

   # /blog/templates/blog/blogs.html
    
    <html>
    <body>
    <h2><a href='/blog/edit_blogs/'>Add New Blog</a></h2>
    {% if blogs %}
        <h2><a href='/blog/edit_articles/'>Add New Article</a></h2>
        <ul>
        {% for blog in blogs %}
            <li>{{ blog.name}} <a href='/blog/edit_blogs/{{ blog.id }}'>[Edit]</a></li>
                <ul>
                {% for article in blog.article_set %}
                    <li><a href='/blog/article/{{ article.id }}'>{{ article.title }}</a> <a href='blog/edit_article/{{ blog.id}}/{{ article.id }}'>[Edit]</a></li>
                {% endfor %}
                </ul>
        {% endfor %}
        </ul>
    {% else %}
        <p>No Blogs are available.</p>
    {% endif %}
    </body>
    </html>


Slide 11
-----------------

**Testing the new view and form**

Test the new view

- Activate the virtual environment
- Run the Django test server using the command python manage.py runserver
- Open the web browser and go to http://127.0.0.1:8000/blog/edit_blog/1
- You will be taken to the new view with the form.