# What is python virtualenv?
A virtual environment helps us isolate our project from our system. It prevents unwanted interference of globally installed packages and programs in our project.
- We use the terminal command `python -m venv myenv` to create a virtual environment called "myenv" in the directory. 
- An isolated python interpreter and pip is installed in this myenv folder and it is isolated from other system software.
- Note: the virtual environment only isolates the package from global python interpreter and global python packages and not global software like Git.
The command `python -m module_name` is used to run python module. `-m` flag tells python to search for the python file for the given module_name. Not specifying the `-m` flag will tell python to search for module_name.py file in  the directory only.
- To activate the virtual environment, use `myenv\Scripts\activate.bat` command in the windows shell
- To deactivate the virtual environment, use `deactivate` command in windows shell
# What is Django MVT architecture?
MVT (Model-View-Template) is an architecture to organize code in web applications (similar to MVC architecture).
- **Model:** A python class that defines the structure of data stored in the database. Uses Django ORM to perform CRUD operations using python code instead of SQL.
- **Views:** A python function that receives HTTP requests and interacts with the Model as needed. Views receive HTTP requests, fetches or updates data in the Model and passes it on to Templates to render UI or return a JSON response.
- **Templates:** HTML file that defines the UI and uses Django template language to insert dynamic data passed from the View.
# Models and Databases in Django
Model classes in django represents a table in our database. Each attribute represents a field/column in the database and an instance of the model represents a row in the table.
```python
from django.db import models

class Member(models.Model):
	firstname = models.CharField(max_length=255)
	lastname = models.CharField(max_length=255)
```
To apply these models in our databases, we use migrations.
- `python manage.py makemigrations` Generates the migration files based on the changes in our model. Run `python manage.py makemigrations <appname>` after creating a new app to generate initial migrations.
- `python manage.py migrate` Applies the migrations to the database.
Python has 3 types of relations:
- **ForeignKey:** Many-to-one relation
- **OneToOneField:** One-to-One relation
- **ManyToManyField:** Many-to-Many relation
```python
from django.db import models

class Profile(models.Model):
    info = models.CharField(max_length=100)

class Group(models.Model):
    name = models.CharField(max_length=50)

class User(models.Model):
    username = models.CharField(max_length=50)
    # One-to-one: Each user has one profile
    profile = models.OneToOneField(Profile, on_delete=models.CASCADE)
    # Many-to-one: Each user belongs to one group
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    # Many-to-many: Each user can have many friends (who are also users)
    friends = models.ManyToManyField('self')
```

### Django ORM
Django ORM maps model attributes to table fields so we only work with python objects and SQL is handled behind the scenes by Django.
- Each model class maps to a table in database
- Each model field maps to a column in database
- Each model class instance maps to a row in database
Django ORM (Object Relational Mapper) provides methods for CRUD (Create, Read/Retrieve, Update, Delete) operations in our database.
For the model file:
```python
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.CharField(max_length=100)
    published_year = models.IntegerField()
```
The Django ORM code will be:
```python
# Import the model
from myapp.models import Book

# --- CREATE ---
book = Book.objects.create(title="Django Basics", author="Alice", published_year=2024)

# --- RETRIEVE ---
all_books = Book.objects.all()                        # Get all books
one_book = Book.objects.get(id=book.id)               # Get a book by ID
filtered_books = Book.objects.filter(author="Alice")  # Filter books by author
first_book = Book.objects.first()                     # Get the first book
count_books = Book.objects.count()                    # Count all books

# --- UPDATE ---
one_book.title = "Django Advanced"
one_book.save() # Save changes

# --- DELETE ---
one_book.delete()  # Delete the book
```
These CRUD operations are used extensively in Views.
# Django vs DRF
Django is focused on full-stack web development building a backend that provides HTML responses (templates) to incoming request, whereas DRF (Django REST Framework) is a framework built on top of Django that is used to make REST APIs using Django and is generally used with React, Angular and other frontend frameworks.
# Views and URLs
Views are python functions that take in request and gives a response based on the models. They use Django ORM to perform CRUD operations on the database according the request.

***Serializers:*** Serializers are used to convert JSON into python objects when data is passed from frontend to backend and from python object to JSON when data is passed from backend to frontend.
The fields that we want to serialize in our JSON object are mentioned in the Meta class inside our serializer along with the model. We can perform validations outside the Meta class in our serializer.
Validations are of 2 type: Built-in validation and Custom Field-Level validation.

```python
class BookSerializer(serializers.ModeSerializer)
	# We want isbn to be unique to all the books
	# We use built-in validator: UniqueValidator
	# General syntax: <field_name> = serializer.<Field_Type>(...)
	isbn = serializer.CharField(
		max_length=13,
		validators=[UniqueValidator(queryset=Book.objects.all())]
	)

	# Meta class defines the fields and the model of the serializer
	class Meta:
		model = Book
		fields = ['id','title','author','published_year','isbn']

	# We want to verify that published year is not in future
	# We use custom field level validation
	# General syntax: def validate_<field_name>
	def validate_published_year(self, value):
		if value > datetime.now().year:
			raise seraizliers.ValidateError("Future publish year")
		return value
```

Views in DRF can be of 3 types: Function Based Views (FBVs), Class Based Views (CBVs) and ViewSets.

views.py:
```python
from rest_framework.decorators import api_view
from rest_framework.views import APIView
from rest_framework.response import Response

# Function Based View (FBV)
@api_view(['GET','POST'])
def list_users(request):
	if request.method == 'GET':
		return Response(["alice","bob","mike"])

# Class Based View (CBV)
class ListUsers(APIView):
	def get(self, request, format=None)
		return Response(["alice","bob","mike"])
```

We can make corresponding paths using urls,
urls.py:
```python
from django.urls import path, include
from .views import list_users, ListUsers

urlpatterns = [
	path('fbv_list/', list_users),
	path('cbv_list/', ListUsers.as_view()),
]
```

ViewSets helps us eliminate boiler plate and provides us more functionality with less code. Unlike APIViews, ViewSets don't map HTTP methods like GET and POST but instead provide actions like `.list(), .create(), .retrieve(), .update()` and `.destroy()`. Viewsets are of 4 types:
1. ViewSet: Very basic, we define all actions manually
2. GenericViewSet: Adds generic behaviour combined with mixins
3. ReadOnlyModelViewSet: Only list and retrieve actions
4. ModelViewSet: All CRUD actions
Routers are classes in DRF which automatically generate URL patterns for our ViewSets.

views.py:
```python
from rest_framework import viewsets
from .models import Book
from .serializers import BookSerializer

class BookViewSet(viewsets.ModelViewSet):
    queryset = Book.objects.all()       # queryset points to the model
    serializer_class = BookSerializer   # provides serializer
```

urls.py (books app):
```python
from rest_framework.routers import DefaultRouter
from .views import BookViewSet

router = DefaultRouter() # Can also be SimpleRouter
router.register(r'books', BookViewSet, basename='book')

urlpatterns = router.urls
```

urls.py (main):
```python
from django.urls import path, include

urlpatterns = [
    path('api/', include('book_app.urls')),
]
```

| URL Pattern        | HTTP Method | Action         | Example Response Data                   |
| ------------------ | ----------- | -------------- | --------------------------------------- |
| `/api/books/`      | GET         | list           | `[{"id":1,"title":"Book A",...}, ...]`  |
| `/api/books/`      | POST        | create         | `{"id":2,"title":"New Book",...}`       |
| `/api/books/<id>/` | GET         | retrieve       | `{"id":1,"title":"Book A",...}`         |
| `/api/books/<id>/` | PUT         | update         | `{"id":1,"title":"Updated Title",...}`  |
| `/api/books/<id>/` | PATCH       | partial_update | `{"id":1,"title":"Partial Update",...}` |
| `/api/books/<id>/` | DELETE      | destroy        | (empty response, status 204)            |
# JWT Authentication
JSON Web Token (JWT) is a token-based authentication mechanism used to secure APIs. A JWT consists of 3 things:
- **Header:** Contains metadata about token signing algorithm and token type.
- **Payload:** Contains data about user like user ID, expiration time, etc.
- **Signature:** Ensures the token hasn't been tempered with (generated by signing the header and payload with a secret key)
How JWT works:
- User sends a sign in request to the server passing their credentials.
- If the credentials are valid, the API sends a JWT token which is stored in local storage by the user. 
- Whenever the user makes an API call, the user sends the JWT token along with the request as a proof of their identity.
- If the token expires, it will have to be refreshed.
# Websockets
# Apps in Django and managing settings.py / manage.py
- Allowing CORS