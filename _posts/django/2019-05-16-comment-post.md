# 댓글기능 구현하기

1. models.py
```python
class Comment(models.Model):
    # TODO : 댓글 남기기를 위해서 form이 필요
    # TODO : Document의 뷰에서 처리
    document = models.ForeignKey(Document, on_delete=models.CASCADE, related_name='comments')
    author = models.ForeignKey(get_user_model(), on_delete=models.SET_NULL, null=True, blank=True, related_name='comments')
    text = models.TextField()
    created = models.DateTimeField(auto_now_add=True)
    updated = models.DateTimeField(auto_now=True)

    def __str__(self):
        return (self.author.username if self.author else "무명") + "의 댓글"
```

2. forms.py
```python
class CommentForm(forms.ModelForm):
    class Meta:
        model = Comment
        fields = ['text']
        
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields['text'].label = "Comment"
```

### 1번째 방법
3. views.py
```python
from .forms import CommentForm

def document_detail(request, document_id):
    # document = Document.objects.get(pk=document_id)
    document = Document.objects.get(pk=document_id)

    # 만약 post 일 때만 댓글 입력에 관한 처리를 더한다.
    if request.method == "POST":
        comment_form = CommentForm(request.POST)
        comment_form.instance.author_id = request.user.id # 작성자
        comment_form.instance.document_id = document_id
        if comment_form.is_valid():
            comment = comment_form.save()

    comment_form = CommentForm()
    comments = document.comments.all()
    return render(request, 'board/document_detail.html', {'object':document, 'comments':comments, 'comment_form':comment_form})
```

4. template
- detail.html 에 내용 추가
```html
<form action="" method="post">
    {% csrf_token %}
    {{comment_form.as_p}}
    <input type="submit" value="comment" class="btn btn-outline-secondary">
    </form>

    <table class="table table-striped">
        <thead>
            <tr>
                <th colspan="3" class="align-left">댓글목록</th>
            </tr>
        </thead>

        <tbody>
        {% for comment in comments %}
            <tr>
                <td>{{comment.text}}</td>
                <td>{{comment.author.username}}</td>
                <td>{{comment.created}}</td>
            </tr>
        {% endfor %}
        </tbody>
     </table>
```

### 2번째 방법 : 댓글 템플릿을 분리
1. views.py
```python
from .forms import CommentForm

def document_detail(request, document_id):
    document = Document.objects.get(pk=document_id)
    comment_form = CommentForm()
    comments = document.comments.all()
    return render(request, 'board/document_detail.html', {'object':document, 'comments':comments, 'comment_form':comment_form})

def comment_create(request, document_id):
    document = get_object_or_404(Document, pk=document_id)
    comment_form = CommentForm(request.POST)
    comment_form.instance.author_id = request.user.id
    comment_form.instance.document_id = document_id
    if comment_form.is_valid():
        comment = comment_form.save()

    # redirect(reverse('board:detail', args=[document_id]))
    return redirect(document)
```

2. comment_list.html
```html

<div class="row">
    <div class="col"></div>
    <div class="col-6">
        <table class="table table-striped">
            <thead>
            <tr>
                <th colspan="3" class="align-left">댓글목록</th>
            </tr>
            </thead>

            <tbody>
            {% for comment in comments %}
            <tr>
                <td>{{comment.text}}</td>
                <td>{{comment.author.username}}</td>
                <td>{{comment.created}}</td>
            </tr>
            {% endfor %}
            </tbody>
        </table>
    </div>
    <div class="col"></div>
</div>

```

3. board / urls.py
```python
from django.urls import path
from .views import *

app_name = 'board'

urlpatterns = [
    path('create/<int:document_id>/', comment_create, name='comment_create'),
]
```

4. template
- comment_list.html
```html
<form action="" method="post">
    {% csrf_token %}
    {{comment_form.as_p}}
    <input type="submit" value="comment" class="btn btn-outline-secondary">
    </form>

    <table class="table table-striped">
        <thead>
            <tr>
                <th colspan="3" class="align-left">댓글목록</th>
            </tr>
        </thead>

        <tbody>
        {% for comment in comments %}
            <tr>
                <td>{{comment.text}}</td>
                <td>{{comment.author.username}}</td>
                <td>{{comment.created}}</td>
            </tr>
        {% endfor %}
        </tbody>
     </table>
```

- detail.html 아래에 내용 추가
```html
<div id="docs_comment_list_area">
    {% include 'board/comment_list.html' %}
</div>
```

## 수정, 삭제
### 페이지에 접근했을 때 구동되는 로직들
[함수형 뷰]
1. 해당 객체가 있는지 확인 - get_object_or_404, objects.get, objects.filter.exists
2. 객체에 대한 권한 체크 - 작성자, 관리자
3-1. get -> 해당 페이지에 필요한 값 입력 받기(form)
3-2. post -> 입력받은 값에 대한 처리 -> 삭제, 업데이트
4. 처리 후 페이지 이동

[클래스형 뷰]
```python
def dispatch(self, request, *args, **kwargs):
    object = self.get_object()
    # 권한 체크
    # super().dispatch(request, *args, **kwargs)
    
    if request.method == "POST":
        # super().post(request, *args, **kwargs)
    else:
        # super().get(request, *args, **kwargs)
```
1. 객체에 대한 권한 체크 - 작성자, 관리자 - dispatch
2. 해당 객체가 있는지 확인 - get_object_or_404, objects.get, objects.filter.exists
- get_object, get_queryset
3-1. get -> 해당 페이지에 필요한 값 입력 받기 - def get
3-2. post -> 입력받은 값에 대한 처리 -> 삭제, 업데이트 - def post
4. 처리 후 페이지 이동

### 코드
- views.py
```python
from django.contrib import messages
def comment_update(request, comment_id):
    comment = get_object_or_404(Comment, pk=comment_id)
    document = get_object_or_404(Document, pk=comment.document.id)
    # user.is_staff
    # user.is_superuser
    if request.user != comment.author:
        messages.warning(request, "권한 없음")
        return redirect(document)

    if request.method == "POST":
        form = CommentForm(request.POST, instance=comment)
        if form.is_valid():
            form.save()
            return redirect(document)
    else:
        form = CommentForm(instance=comment)
    return render(request, 'board/comment/comment_update.html',{'form':form})

def comment_delete(request, comment_id):
    comment = get_object_or_404(Comment, pk=comment_id)
    document = get_object_or_404(Document, pk=comment.document.id)

    if request.user != comment.author and not request.user.is_staff and request.user != document.author:
        messages.warning(request, "권한 없음")
        return redirect(document)

    if request.method == "POST":
        comment.delete()
        return redirect(document)
    else:
        return render(request, 'board/comment/comment_delete.html',{'object':comment})

```

- urls.py
```python
from django.urls import path
from .views import *

app_name = 'board'

urlpatterns = [
    path('comment/update/<int:comment_id>/', comment_update, name='comment_update'),
    path('comment/delete/<int:comment_id>/', comment_delete, name='comment_delete'),
]
```

- template
1. update
```html
{% extends 'base.html' %}
{% block content %}
<form action="" method="post">
    {% csrf_token %}
    {{form.as_p}}
    <input type="submit" value="Update" class="btn btn-outline-warning">
</form>
{% endblock %}
```

2. delete
```html
{% extends 'base.html' %}
{% block content %}
<form action="" method="post">
    <div class="alert alert-danger">Do you want to delete {{object}}?</div>
    {% csrf_token %}
    {{form.as_p}}
    <input type="submit" value="Delete Confirm" class="btn btn-outline-danger">
</form>
{% endblock %}
```
