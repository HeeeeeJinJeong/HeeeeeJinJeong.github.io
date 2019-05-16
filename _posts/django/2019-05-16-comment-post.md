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

