# django에서 DB 생성

## 1. 어떠한 데이터베이스를 사용할 것인지 설정해 주어야 한다.

## 2. 테이블에 대한 형태를 정의해 주어야 한다.

- 테이블에 대한 정의는 app안에 있는 models.py에서 정의됩니다.

- models.py에서 모델을 만들어야 하는데, 모델은 데이터베이스에서의 테이블과 같다.

- 앞으로 우리가 Todo에 대한, 데이터를 다룰 것이기 때문에

```python
#models.py
class ToDo(models.Model):
    content = models.CharField(max_length=255)

```

- ToDo라는 객체(테이블)에 content라는 속성을 정의했다.

## 3. Migration을 만들자

- 우리가 만든, models.py를 가지고 데이터베이스에 테이블을 만들어야 한다.

- 그렇게 하기 위한, 첫번째 단계가 'Migration'

### 만드는 이유
- 알다시피, 위에 있는 코드만으로는 데이터베이스의 테이블을 생성하는 데, 많이 부족하다.
- 따라서 사용자의 의도인 modles.py를 DB가 알아 듣도록, 구체화되어 있는 설계도를 만드는 것이다.

```

python manage.py makemigrations

```

## 4. Migration으로 테이블을 만들자.

```
python manage.py migrate



```
## 5. 테이블 확인

```
python manage.py dbshell
.tables
my_to_do_app_ToDo

pragma table_info(my_to_do_app_ToDo)
select * from my_to_do_app_toDo;

```
- DB에 저장되어 있는 규칙이 있는 (applicatino_name)_테이블

<br>

# Form을 이용한 사용자의 정보 전달하기

## 1. html

- html에서 사용자가 전달하는 정보를 전달받고 싶다면, form 태그로 감싸야 한다.
  
```html

<form action = "./createTodo/" method = "POST">{% csrf_tokens %}

```
- send를 하게 됨녀, 위 주소로 POST 방식으로 값을 전달한다.
- POST 방식으로 값을 전달 받을 려면, {% csrf_tokens %}필수이다.


## 2. ./createTodo가 향하는 곳

1. ToDolist > url.py: 기본 프로젝트의 url을 지나간다.
2. my_to_do_app > url.py: ./createTodo가 들어왔을 때, 어떠한 행동을 해야 하는 지 알려줘야 한다.

```python

#urls.py
urlpatterns = [
    path('', views.index, name = 'index'),
    path('createTodo/', views.createTodo, name = 'createTodo')
]

# views.py
def createTodo(request):
    user_input_str = request.POST['todoContent']
    new_todo = ToDo(content = user_input_str)
    new_todo.save()
    return HttpResponseRedirect(reverse('index'))

```

- 이러한 느낌으로 원하는 동작을 client에 보내준다.

## 3. 데이터 저장
> 위의 코드를 받아서.
- 우리가 send 명령을 하면, form태그 안에 있는 client가 입력한 정보들이 request 객체에 쌓여서 Django로 들어오게 된다.

- request 객체는 urls.py를 따라서 views.py로 오게 된다.
- request 객체는 name 값으로 서로를 구별한다. 내가 저장하고 싶은 정보는 name = 'todoContent'이므로 request.Post['todoContnet]를 통해서, 정보를 전달한다.

- migration을 했던, ToDo객체에 원소값으로 전달하고 save()를 진행한다.

- 원한다면, name이 index인 url로 redirect를 할 수 있다.

# 데이터베이스에 저장된 정보를 보여주는 과정
> 첫화면이 DB의 테이블의 값을 전달받고 싶다.


1. 첫화면의 url은?
- 아무것도 입력하지 않는 Default이다.

2. 그렇다면 urls와 views.py는

```python

    #urls.py
    path('', views.index, name = 'index')


    #views.py
    def index(request):
    #return HttpResponse("my_to_do_app first page")

    todos = ToDo.objects.all()
    content = {'todos' : todos}
    return render(request, 'my_to_do_app/index.html', content)

```
- 우리는 ToDo라는 객체(DB에서는 테이블)을 만들었다. 그 객체를 통해서 정보를 얻어야 한다.
- 정보를 가진 todos를 render(템플릿을 불러오는 함수)를 통해 content도 html에 보낸다.


## 1. html
- 장고에서는 딕셔너리의 이름을 명시할 필요가 없다. 따라서, content라는 딕셔너리명을 쓸 필요는 없다.
- key Value값인 'todos'를 활용하면 된다.


```html

<% for todo in todos %> # client에 보여주고 싶지 않는

{{todo.content}} # 보여줄
{{todo.id}}
<% endfor %>

```
- 위 코딩을 통해서 DB의 데이터를 html에 뿌리면 된다.

