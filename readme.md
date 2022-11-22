# How to Create CRUD API in Django?

![Endpoint](../master/django-crud-api.png)

In this tutorial, you will learn how to create crud api in django. let’s discuss about crud django api using django and django rest framework. I would like to share with you django crud api example. We will use how to create rest crud api in django rest framework. you will do the following things for django rest crud api example tutorial.

In this tutorial we'll create a basic Django To Do app and then convert it into a web API using serializers, viewsets, and routers.

# Step 1: Create a Project

In this step, we’ll create a new django project using the django-admin. Head back to your command-line interface and run the following command:

`django-admin startproject example`

# Step 2: Create a App

Now we'll create a single app called todo to store a list of post names. We're keeping things intentionally basic. Stop the local server with Control+c and use the startapp command to create this new app.

`python manage.py startapp todo`

# Step 3: Update setting.py

In this step we require to do two things in our settings.py file, One is our installed app name Add the below lines to your settings.py file:

Next, you need to add it in the settings.py file as follows:

example/settings.py


....
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'todo',
]



# Step 4: Create a Model

In this step we will require the database model for storing contacts.Open the todo/models.py file and add the following code:

todo/models.py

from django.db import models


class Todo(models.Model):
    title = models.CharField(max_length = 100)
    body = models.CharField(max_length = 100)
    is_completed = models.BooleanField(default=False)
    date_created = models.DateField(auto_created=True)
    last_modified = models.DateField(auto_now=True)

    def __str___(self):
        return self.title



Ok, all set. We can engender a migrations file for this change, then integrate it to our database via migrate.

python manage.py makemigrations
python manage.py migrate


# Step 5: Creating the Serializers

In this step, we need to create Serializers allow complex data such as querysets and model instances to be converted to native Python datatypes that can then be easily rendered into JSON, XML or other content types. Serializers also provide deserialization, allowing parsed data to be converted back into complex types, after first validating the incoming data. Let’s start creating a serializer.

todo/serializers.py


from rest_framework import serializers
from todo.models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = "__all__"


# Step 6: Creating the Views

In this step, we need to create the views for performing the fetch record to the database.Open the todo/views.py file and add:

todo/views.py


from django.shortcuts import render
from rest_framework.generics import ListAPIView
from rest_framework.generics import CreateAPIView
from rest_framework.generics import DestroyAPIView
from rest_framework.generics import UpdateAPIView
from todo.serializers import TodoSerializer
from todo.models import Todo


# Create your views here.

class ListTodoAPIView(ListAPIView):
    """This endpoint list all of the available todos from the database"""
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer

class CreateTodoAPIView(CreateAPIView):
    """This endpoint allows for creation of a todo"""
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer

class UpdateTodoAPIView(UpdateAPIView):
    """This endpoint allows for updating a specific todo by passing in the id of the todo to update"""
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer

class DeleteTodoAPIView(DestroyAPIView):
    """This endpoint allows for deletion of a specific Todo from the database"""
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer



# Step 7: Creating URLs

In this section, we need a urls.py file within the apis app however Django doesn't create one for us with the startapp command. Create todo/urls.py with your text editor and paste below code.

todo/urls.py


from django.urls import path
from todo import views

urlpatterns = [
    path("",views.ListTodoAPIView.as_view(),name="todo_list"),
    path("create/", views.CreateTodoAPIView.as_view(),name="todo_create"),
    path("update/<int:pk>/",views.UpdateTodoAPIView.as_view(),name="update_todo"),
    path("delete/<int:pk>/",views.DeleteTodoAPIView.as_view(),name="delete_todo")
]



Next, we will require the modify the urls.py your root preoject folder lets update the file.


example/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/todo/',include("todo.urls"))
]


# Run the Server

In this step, we’ll run the local development server for playing with our app without deploying it to the web.

`python manage.py runserver`


# Postman POST request:

To create a new Todo we make a POST request to http://localhost:8000/api/v1/todo/create/ with the new Todo object.

{
    "date_created": "2022-11-03",
    "title": "Django",
    "body": "Django is a Awesome",
    "is_completed": False
}


![Endpoint](../master/create-req.png)

# Postman GET request:
Making a GET request to http://localhost:8000/api/v1/todo in postman returns a list of todos.

[
    {
        "id": 4,
        "date_created": "2022-11-03",
        "title": "Django",
        "body": "Django is a Awesome",
        "is_completed": true,
        "last_modified": "2022-11-03"
    }
]


![Endpoint](../master/get-req.png)

# Postman PUT request:

To Update a Todo we make a PUT request to http://localhost:8000/api/v1/todo/update/4/ with the Todo object fields to update and pass in Todo ID as a URL parameter.

{
    "date_created": "2022-11-03",
    "title": "Python",
    "body": "Python is a Awesome",
    "is_completed": False
}



![Endpoint](../master/put-req.png)

# Postman DELETE request:

To Delete a Todo we make a DELETE request to http://localhost:8000/api/v1/todo/delete/4/ passing the ID of the Todo to delete as URL parameter.



![Endpoint](../master/delete-req.png)



## Thank You
