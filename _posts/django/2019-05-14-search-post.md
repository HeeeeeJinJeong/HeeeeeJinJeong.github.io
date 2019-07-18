# 검색기능 구현하기

1. 검색어
2. filter
- 어떤 항목? : 제목, 내용, 작성자 ...
- 어떤 옵션? : 대소문자 구분 여부, 키워드 시작점, 끝나는 지점 ...


## filter
### .filter를 많이 써도 실제 값을 요청하기 전까지는 쿼리문이 실행되지 않음
- 필드명 = "값" 매칭
- 필드명_exact = "값" 매칭
- 필드명_iexact = "값" 대소문자 구분 없이 매칭
- __startswith, __istartswith 값으로 시작
- __endswith, __iendswith 값으로 끝
- __continas, __icontains 값을 포함하느냐

### ForeignKey 매칭
- 필드명__해당모델의필드명 = 매칭
- 필드명__해당모델의필드명__옵션 = 위와 동일하게 동작

- __gt=값, __gte=값 크다, 크거나 같다.
- ex) created__gt = 오늘 -> 작성일이 오늘보다 크다

- __lt=값, __lte=값 작다, 작거나 같다.
- ex) created__lt = 오늘 -> 작성일이 오늘보다 이전
- ex) 판매시작일__lte = 오늘 -> 판매시작일 설정값이 오늘보다 작거나 같으면 판매 시작

## Q
- objects.filter() : filter 메서드에 들어가는 매개변수들은 항상 and 연산을 한다.
- or 연산을 하고싶어서 Q 객체를 사용한다. (사용법은 filter 에 들어가는 매개변수의 작성법과 똑같다.)
- Q() | Q() -> or
- Q() & Q() -> and
- ~Q() -> not

### 옵션
- username : Q(author__username__icontains=search_key)
- title : Q(title__icontains=search_key)
- text : Q(text__icontains=search_key)
- q1 | q2 | q3
```python
if search_key and search_type:
    documents = get_list_or_404(Document, q1|q2|q3)
```

## F
from django.db.models import F

###
- title이 text에 포함되어 있는지 확인 : Document.objects.filter(text__icontains=F('title'))
- text에 유저이름이 포함되어 있는지 확인 : Document.objects.filter(text__icontains=F('author__username'))

## exclude() 제외
- Document.objects.exclude(title__icontains='sd') : title에 'sd' 가 들어있는 포스트를 제외하고 출력
- Document.objects.exclude(category__in=Category.objects.filter(name__icontains='dubu')) : 'dubu' 카테고리 제외하고 출력

### exclude 옵션
1. 블로그 제목에 제외 키워드가 있는 경우
2. 블로그 생성일이 한달 이내인 경우
- Document.objects.exclude(blog__in=Blog.objects.filter(title__icontains='key', created__gt=datetime.now()-timedelta(months=1))
3. 나머지 글들만


### QuerySet
```python
# 1) QuerySet의 기본 정렬값 : 모델에서 설정값
Model:
    class Meta:
        ordering = ['field name']
        
# 2) 기본 정렬값은 pk
Document.objects.all().order_by('title') 오름차순 - asc
Document.objects.all().order_by('-title') 내림차순 - desc

# delete
Document.objects.get(pk=1).delete()

# update
Document.objects.get(pk=1).update(title="dd")

# 전체 레코드 갱신
Document.objects.all().update(title="dd")
Document.objects.filter(title__icontains='word').update(title="dd")
```


```shell
>>> from django.db.models import F, Q
>>> from board.models import *
>>> Document.objects.all()
<QuerySet [<Document: dubu>, <Document: DUBUd>, <Document: sadsdfa>, <Document: sd>, <Document: asdf>, <Document: asdd>]>
>>> Document.objects.filter(title__icontains='as')
<QuerySet [<Document: asdf>, <Document: asdd>]>
>>> Document.objects.filter(text__icontains=F('title'))
<QuerySet [<Document: asdd>]>
>>> Document.objects.filter(text__icontains=F('author__username'))
<QuerySet []>
```

```shell
(venv) ➜  board_project git:(master) ✗ python manage.py shell
Python 3.7.1 (v3.7.1:260ec2c36a, Oct 20 2018, 03:13:28) 
[Clang 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from board.models import *
>>> Document.objects.filter(pk=4)
<QuerySet [<Document: sd>]>
>>> Document.objects.filter(pk=4, title='sd')
<QuerySet [<Document: sd>]>
>>> Document.objects.filter(title__startswith='sd')
<QuerySet [<Document: sd>]>
>>> Document.objects.filter(title__startswith='as')
<QuerySet [<Document: asdf>, <Document: asdd>]>
>>> Document.objects.filter(title__endswith='d')
<QuerySet [<Document: DUBUd>, <Document: sd>, <Document: asdd>]>
>>> >>> Document.objects.filter(title='dubu')
<QuerySet [<Document: dubu>]>
>>> Document.objects.filter(title__iexact='dubu')
<QuerySet [<Document: dubu>]>
>>> Document.objects.filter(title__icontains='sd')
<QuerySet [<Document: sadsdfa>, <Document: sd>, <Document: asdf>, <Document: asdd>]>
>>> Document.objects.filter(category__name='dubu')
<QuerySet [<Document: asdd>, <Document: asdf>, <Document: sd>, <Document: sadsdfa>, <Document: DUBUd>, <Document: dubu>]>
>>> Document.objects.filter(category__name__endswith='d')
<QuerySet []>
```

### Document.objects.prefetch_related('author').all()
1) select_related -> join query를 만들어서 한큐에 데이터를 불러온다.
- ForeignKey까지만 묶을 수 있다.

2) prefetch_related -> Document 부르고, Category 불러서 -> 코드단에서 병합
- ManyToMany 도 지원

3. select_related, prefetch_related 를 사용하지 않는 경우
- 참조 테이블에 대한 데이터 질의를 항상 실행한다(DB에 부담이 높음)

### views.py
```python
def document_list(request):
    # QuerySet
    # 1. 객체 선택
    # 2. 객체 생성
    # 3. 객체 필터링
    # 4. 객체 삭제

    # 페이징 기능 구현
    # 현재 페이지 번호
    page = int(request.GET.get('page',1))
    # 페이지당 갯수
    paginated_by = 3

    # 1. 모델의 전체 데이터 불러오기
    # documents = Document.objects. all()
    # documents = get_list_or_404(Document)

    search_type = request.GET.getlist('search_type', None)
    # request.GET.get -> 아이템 1
    # request.GET.getlist -> 리스트 형태로


    search_q = None
    search_key = request.GET.get('search_key', None)

    if search_key and search_type:
        if 'title' in search_type:
            search_q = Q(title__icontains=search_key)
            # search_q = search_q | temp_q if search_q else temp_q
        if 'username' in search_type:
            search_q = Q(author__username__icontains=search_key)
            # search_q = search_q | temp_q if search_q else temp_q
        if 'text' in search_type:
            search_q = Q(text__icontains=search_key)
            # search_q = search_q | temp_q if search_q else temp_q
        if 'all' in search_type:
            search_q = Q(title__icontains=search_key) | Q(author__username__icontains=search_key) | Q(text__icontains=search_key)

        documents = get_list_or_404(Document, search_q)
    else:
        documents = get_list_or_404(Document)

    # QuerySet 객체를 슬라이싱 할 때 [시작번호:끝번호]
    # 1페이지 - 0부터 ========== page*paginated_by
    # 2페이지 - paginated_by*(page-1) ========== page*paginated_by
    total_count = len(documents)
    total_page = math.ceil(total_count/paginated_by)
    page_range = range(1, total_page+1)

    # 전체 갯수 -> 전체 페이지 수
    start_index = paginated_by*(page-1)
    end_index = page*paginated_by

    documents = documents[start_index:end_index]
    return render(request, 'board/document_list.html', {'object_list':documents, 'total_page':total_page, 'page_range':page_range})
```

### document_list.html
```html
<form action="" method="get" id="search_form" class="form-inline justify-content-center">
    <select class="custom-select custom-select-sm" style="margin-right:1em;" name="search_type">
        <option value="all">All</option>
        <option value="username">작성자</option>
        <option value="title">글 제목</option>
        <option value="text">글 내용</option>
    </select>

    <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search" name="search_key">
    <button class="btn btn-outline-dark my-2 my-sm-0" type="submit">Search</button>
</form>
```

### post/views.py
```python
def post_list(request, page):
    # posts = get_list_or_404(Post)

    # 페이지 번호 얻기
    page = page if page else 1
    page = int(page)

    queryset = Post.objects.all()  # CBV : get_queryset

    # post으로 전달된 데이터 중에서 search 변수값 찾기
    search_keyword = request.POST.get('search', request.GET.get('search', None))
    context_data = {}
    if search_keyword:
        q = Q(text__icontains=search_keyword)
        q |= Q(title__icontains=search_keyword)
        queryset = queryset.filter(q)
        context_data.update({'search_keyword': search_keyword})

    # 페이징 처리
    paginator = Paginator(queryset, 3)
    page = paginator.page(page)
    context_data.update({'object_list': page.object_list,
                         'paginator': paginator, 'page_obj': page, 'is_paginated': True})

    return render(request, 'post/post_list.html', context_data)
```
### post_list.html
```html
{% if is_paginated %}

    <ul class="pagination justify-content-center pagination-sm">
        {% if page_obj.has_previous %}
        <li class="page-item"><a
                href="{% url 'post:list' page_num %}{% if search_keyword %}?search={{search_keyword}}{% endif %}"
                class="page-link">이전페이지</a></li>
        {% else %}
        <li class="page-item disabled"><a href="#" class="page-link">이전페이지</a></li>
        {% endif %}

        {% for page_num in paginator.page_range %}
        <li class="page-item {% if page_obj.number == page_num %}disabled{% endif %}"><a
                href="{% url 'post:list' page_num %}{% if search_keyword %}?search={{search_keyword}}{% endif %}"
                class="page-link">{{page_num}}</a></li>
        {% endfor %}

        {% if page_obj.has_next %}
        <li class="page-item"><a
                href="% url 'post:list' page_num %}{% if search_keyword %}?search={{search_keyword}}{% endif %}"
                class="page-link">다음페이지</a></li>
        {% else %}
        <li class="page-item disabled"><a href="#" class="page-link">다음페이지</a></li>
        {% endif %}
    </ul>

    {% endif %}
```
