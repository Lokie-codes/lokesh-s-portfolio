---
title: Mastering API Viewsets in DRF - A Comprehensive Guide with Practical Examples
date: 2022/6/23
description: API Viewsets are a powerful feature in Django Rest Framework (DRF)
tag: web development
author: You
---


### Introduction:
API Viewsets are a powerful feature in Django Rest Framework (DRF) that streamline the development of CRUD (Create, Retrieve, Update, Delete) operations for your APIs. They provide a consistent and efficient way to handle common API actions, reducing boilerplate code and promoting code reusability. In this article, we will explore the concept of API Viewsets in DRF and dive into practical examples to illustrate their usage.

### Understanding API Viewsets:
API Viewsets in DRF are classes that combine multiple actions into a single interface. They represent a set of related operations on a model or resource. Each action corresponds to a specific HTTP method and performs a specific task, such as creating, retrieving, updating, or deleting resources. DRF provides different types of viewsets to cater to varying needs.

### ModelViewSet:
ModelViewSet is one of the most commonly used viewsets in DRF. It automatically generates CRUD operations for a model-based resource. Let's see an example:
```
from rest_framework import viewsets
from .models import Book
from .serializers import BookSerializer

class BookViewSet(viewsets.ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

In this example, we define a BookViewSet that extends the ModelViewSet class. We specify the queryset to fetch all books and the BookSerializer to serialize the data. This single viewset provides actions like list, create, retrieve, update, and destroy for the Book model.

### ReadOnlyModelViewSet:
ReadOnlyModelViewSet is useful when you want to provide read-only access to a model-based resource. It generates only read-only actions like list and retrieve, disabling creation, updating, and deletion. Here's an example:
```
from rest_framework import viewsets
from .models import Book
from .serializers import BookSerializer

class BookViewSet(viewsets.ReadOnlyModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

In this case, the BookViewSet provides read-only actions, allowing users to list all books and retrieve a specific book.

### Custom Actions:
API Viewsets also allow you to define custom actions beyond the standard CRUD operations. These actions can be triggered by custom HTTP methods or be associated with specific URLs. Let's see an example:
```
from rest_framework import viewsets
from rest_framework.decorators import action
from rest_framework.response import Response

class BookViewSet(viewsets.ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer

    @action(detail=True, methods=['post'])
    def borrow(self, request, pk=None):
        book = self.get_object()
        # Perform borrowing logic here
        return Response({'message': 'Book borrowed successfully'})

    @action(detail=False)
    def bestsellers(self, request):
        bestsellers = Book.objects.filter(bestseller=True)
        serializer = self.get_serializer(bestsellers, many=True)
        return Response(serializer.data)
```

In this example, we define two custom actions: 'borrow' and 'bestsellers'. The 'borrow' action allows users to borrow a book by sending a POST request to the corresponding URL. The 'bestsellers' action lists all the best-selling books by sending a GET request to the associated URL.

### Registering Viewsets in URLs:
To use API Viewsets, we need to register them in the URLs of our Django application. We can do this using routers or by manually defining the URL patterns. Here's an example using routers:
```
from django.urls import include, path
from rest_framework import routers
from .views import BookViewSet

router = routers.DefaultRouter()
router.register('books', BookViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
```

In this case, we create a router and register the BookViewSet with the 'books' endpoint. The router automatically generates the necessary URLs for the viewset actions.

### Conclusion:
API Viewsets in Django Rest Framework simplify the development of CRUD operations for your APIs. By combining multiple actions into a single interface, they promote code reusability, reduce boilerplate code, and provide a consistent structure for handling API operations. Whether you're working with ModelViewSet, ReadOnlyModelViewSet, or custom actions, API Viewsets offer a comprehensive solution for building powerful and efficient APIs.

By mastering API Viewsets in DRF, you can elevate your API development to the next level and create robust, scalable, and maintainable APIs.

Happy coding with Django Rest Framework's API Viewsets!

