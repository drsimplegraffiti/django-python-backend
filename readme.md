#### Setting up IDE

install pipenv: pip install pipenv
install python extension in vscode


###Install pipenv correctly
1. Run Windows PowerShell as Administrator
2. pip install pipenv
3. Set path variable: set PATH=%PATH%;set PATH=%PATH%; 'C:\Users\user\AppData\Local\Programs\Python\Python310\Scripts'



Add this o your Path variable
 c:\users\user\appdata\local\packages\pythonsoftwarefoundation.python.3.10_qbz5n2kfra8p0\localcache\local-packages\python310\Scripts


### create your first django project
Make a directory and cd in to it
install django inside: pipenv install django
pipenv will create 2 files 
1. Pipfile
2. Pipfile.lock

The Pipfile and Pipfile.lock is like the package.json in Javascript

Open the code in vscode using `code .`

Next we  need to activate the virtual environment, to activate run: pipenv shell
To deactivate: Type deactivate or exit or Ctrl + D

Start a new project by running 
> django-admin startproject [project name] .

#### Run server
python manage.py runserver

#### Run server with port number by running 
python manage.py runserver 9000


#### Using Integrated Terminal in Vscode
Press Ctrl + Shift + P and type python interpreter
To get the path to your virtual env run in a new terminal in the project folder: pipenv --venv
It will give something like this: C:\Users\user\.virtualenvs\storefront-ZU67bnU7

Or vscode will give you the path automatically

Next open terminal and you will see that vscode automatically activate the project 
You see something like: 

❯ source C:/Users/user/.virtualenvs/storefront-ZU67bnU7/Scripts/activate


Run the command in your Integrated Terminal: python manage.py runserver 5555



#### Create Django app
A django project consists of several apps
Go to the settings.py and check the INSTALLED_APPS section
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```



Create a new app by running
python manage.py startapp playground

After creating a new app You need to register the app in the setting module

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'playground'
]

```


---

### Writing views
Is just like the request handler
Go to the view and write logic there

```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def say_hello(request):
    return HttpResponse("Hello world")
```


---

#### Mapping views to url
Next we map this view to a url
In your app folder in my case `playground.py` add a new file called urls.py

```python
from django.urls import path
from . import views

# URLConf
urlpatterns = [
    path('hello/', views.say_hello)
]
```

Then go to the urls.py in the main project folder in my case the storefront folder
Add the include function to the import

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('playground/', include('playground.urls'))
]

```

Then if you visit : http://127.0.0.1:5555/playground/hello/
You get Hello world returned 


---

#### Templates in Django
views in Express are called Templates in Django
In the playground folder, add a new folder called template and inside a file called hello.html

Add any html to your taste 

Then go to your views.py and use the render function to render the template

```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def say_hello(request):
    return render(request, 'hello.html')
```


---

### PASSING DYNAMIC VALUES
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def say_hello(request):
    return render(request, 'hello.html', {"name":"Joseph"})
```

In your hello.html
```
<h1>hello {{name}}</h1>
```


---

#### Writing logic
```python

{% if name%}
<h1>hello {{name}}</h1>
{% else %}
<h1>Hello guest</h1>
{% endif %}
```


Debug Django using Django Debug Toolbar
Install in your django project folder: pipenv install django-debug-toolbar

Add the debug_toolbar to the installed_apps unders settings.py
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'playground',
    'debug_toolbar'
]


Next we add this urlpattern in the main url.py config file
```python
from django.contrib import admin
from django.urls import path, include
import debug_toolbar

urlpatterns = [
    path('admin/', admin.site.urls),
    path('playground/', include('playground.urls')),
    path('__debug__/'), include(debug_toolbar.urls)
]
```

Next we add in the middleware
In the seetings.py under the middleware settings add :

```python

MIDDLEWARE = [
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

Next we configure the internal ip address and paste anywhere in the settings.py file

```python
INTERNAL_IPS = [
    # ...
    "127.0.0.1",
    # ...
]
```

To see the toolbar in action, u must return a proper html in the template



--- #### Install code formatter
pipenv install black
  
To use black press CTRL + SHIFT + P and type Workspace Settings(JSON)
Add the following code 
```python
{
    "python.formatting.provider": "black",
    "editor.formatOnSave": true,
}
```


---


#### Installing and connecting PostgresDB
> pipenv install psycopg2

Next we connect to our postgres local by going to the settings.py and scroll to the DATABASES section
and change the Engine from db.sqlite3 to postgres

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql_psycopg2",
        "NAME": "postgres",  # database name
        "USER": "postgres",
        "PASSWORD": "",
        "HOST": "localhost",
        "PORT":"5432"
    }
}
```

#### Next we migrate our database 
> python manage.py makemigrations

#### Apply migration
Next we apply our migrations
> python manage.py migrate


On successful migration, you can check your psql database, open a new terminal 
> psql -U postregres

> \c django1

> \dt
