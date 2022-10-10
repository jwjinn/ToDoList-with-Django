# 장고 프로그램 시작부터 html 등록

## 1. 장고프로젝트 시작

```python

django-admin startproject 프로젝트명(ToDoList)

```
- 폴더명으로 ToDoList가 생성이 된다.
- 패키지명으로 같은 이름의 ToDoList가 생성이 된다.
- manage.py가 생성이 된다.

## 2. Application 생성

```python

python manage.py startapp my_to_do_app

```
- Application을 생성한다.


## 3. Application을 프로젝트에 등록 - Setting.py

```python

INSTALLED_APPS = [
    'my_to_do_app'
]

```
- 실행하는 쪽에 등록을 시작.

## 4. url 등록 - urls.py

```python

from django.contrib import admin
from django.urls import path, include

    path('', include('my_to_do_app.urls')),
```
- 직접 추가한 프로그램은 include함수를 사용해야 한다.
- 위 ''주소로 들어오면, 사용자가 따로 추가한 my_to_do_app에 있는 urls.py로 이동하라고 적어둔것.


## 5. my_to_do_app의 urls.py

```python

from django.urls import path
from . import views

urlpatterns = [
    path('', views.index)
]

```
- 들어왔으니, view.index로 연결시킨다.
- views.index는 클라이언트에게 보여줄 View파일

## 6. my_to_do_app의 view.py

```python

def index(request):
    return HttpResponse("my_to_do_app first page")

```

## 7. my_to_do_app의 html 템플릿 저장

1. my_to_do_app에서 templates 폴더를 생성
2. templates폴더에 app의 이름과 같은 폴더를 생성
3. index.html


## 8. my_to_to_app의 views.py를 설정

```python
def index(request):
    return render(request, 'my_to_do_app/index.html')
```

