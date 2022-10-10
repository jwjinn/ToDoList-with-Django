# Delete Logic

## 1. html에는 form과 id 값을 가지고 있어야 한다.

```html

<form action ="./deleteTodo" method = "GET">

    <input type = "hidden" id = "todoNum" name = "todoNum" value = "{{todo.id}}">
</form>

```

## 2. url과 views.py를 통해서 로직을 처리한다.

```python

def doneTodo(request):

    done_todo_id = request.GET['todoNum']
    print("완료한 todo의 id", done_todo_id)
    todo = ToDo.objects.get(id = done_todo_id)
    todo.delete()
    return HttpResponseRedirect(reverse('index'))

```

- done_todo_id에 html에서의 id값을 저장합니다.
- todo객체에 id값이 동일 한 테이블(객체)에 삭제 명령을 내립니다.