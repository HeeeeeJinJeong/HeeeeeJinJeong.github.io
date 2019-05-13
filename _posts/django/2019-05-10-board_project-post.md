# board_project

### 장고 설치 : pip install django

### 장고 프로젝트 생성 : django-admin startproject config .

### db는 AWS RDS - PostgreSQL 10.x 버전
- ec2 : 리눅스 서버
- RDS : DB(postgre-5432, mysql-3306)
- S3 : file server
- IAM : API Key


1. 아마존 -> RDS -> 파라미터 그룹(한글) -> 데이터베이스 생성
2. pip install psycopg2-binary
3. config / settings.py
```python
# pip install psycopg2-binary
# Will Learn : 멀티 데이터베이스
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME':'wps_board',
        'USER':'heejin',
        'PASSWORD':'',
        'HOST':'', # RDS 엔드포인트
        'PORT':'5432',
    }
}
```


### S3 setting
1. 아마존 -> S3 -> 버킷 만들기(files.jjjinnn.com 도메인 형식으로 만들기) -> 만든 버킷 접속 -> 속성 -> 정적 웹 사이트 호스팅 활성화
2. 아마존 -> route53 -> 레코드 세트 생성 -> 레코드 세트 저장 -> setting.py 에 RDS 엔드포인트 입력
- DB 사용하는 이유 : 대량의 데이터를 다루기 쉽고 빠르게 사용하기 위해서
3. python manage.py migrate
4. python manage.py createsuperuser
5. pip install django-storages
6. pip install boto3
7. config / asset_storage.py
```python
from storages.backends.s3boto3 import S3Boto3Storage

class MediaStorage(S3Boto3Storage):
    location = 'media'
    file_overwrite = False
```

8. settins.py
```python
AWS_ACCESS_KEY_ID = ''
AWS_SECRET_ACCESS_KEY = ''
AWS_REGION = 'ap-northeast-2'
AWS_STORAGE_BUCKET_NAME = 'files.jjjinnn.com'
AWS_S3_CUSTOM_DOMAIN = '%s' % (AWS_STORAGE_BUCKET_NAME)
AWS_S3_SECURE_URLS = False
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
AWS_DEFAULT_ACL = 'public-read'
AWS_LOCATION = 'static'

STATIC_URL = 'http://%s/%s/' % (AWS_S3_CUSTOM_DOMAIN, AWS_LOCATION)
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'

DEFAULT_FILE_STORAGE = 'config.asset_storage.MediaStorage'
```

9. python manage.py collectstatic

10. images.jjjinnn.com 버킷 만들기 -> 정적 웹 사이트 활성화

11. asset_storage.py 수정
```python
from storages.backends.s3boto3 import S3Boto3Storage

class MediaStorage(S3Boto3Storage):
    location = 'media'
    bucket_name = 'images.jjjinnn.com'
    custom_domain = 'images.jjjinnn.com'
    file_overwrite = False
```

-----------------------------------------------------------------------------------------------

- class : 설계도
- instance(객체, object) : 실제 건물

- 모델 폼
- model = Document
- fields = ['']
### app : python manage.py startapp board
- board
- config / settings.py -> INSTALLED_APPS 에 board추가

### model
- Document - author, category, title, slug, text, image, created, updated
- Category - slug, name

```python
from django.db import models

# Create your models here.
from django.contrib.auth import get_user_model
# get_user_model : User 모델을 커스텀 한 경우

class Board(models.Model):
    pass


class Category(models.Model):
    name = models.CharField(max_length=20)
    slug = models.SlugField(max_length=30, db_index=True, unique=True, allow_unicode=True)
    # slug : 주소에 노출
    # db_index : 검색과 정렬이 용이하게 / allow_unicode : 한글사용
    description = models.CharField(max_length=200, blank=True) # 카테고리 설명
    meta_description = models.CharField(max_length=200, blank=True) # 검색엔진에 제공

    class Meta:
        # DB에 기본적으로 설정될 정렬값
        ordering = ['slug']
        
    def __str__(self):
        return self.name

class Document(models.Model):
    category = models.ForeignKey(Category, on_delete=models.SET_NULL, null=True, blank=True, related_name='documents')
    author = models.ForeignKey(get_user_model(), on_delete=models.CASCADE, related_name='documents')
    title = models.CharField(max_length=100)
    slug = models.SlugField(max_length=120, db_index=True, unique=True, allow_unicode=True)
    text = models.TextField()
    image = models.ImageField(upload_to='board_images/%Y/%m/%d') # upload_to 동적으로 경로 설정 가능
    created = models.DateTimeField(auto_now_add=True)
    updated = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.title
```

### python manage.py makemigrations board

### python manage.py migrate 

### admin.py
```python

class CategoryOption(admin.ModelAdmin):
    list_display = ['id', 'name', 'slug']
    prepopulated_fields = {'slug':('name',)} # slug 자동설정

admin.site.register(Category, CategoryOption)

class DocumentOption(admin.ModelAdmin):
    list_display = ['id', 'author', 'title', 'slug', 'created', 'updated']
    prepopulated_fields = {'slug':('title',)}

admin.site.register(Document, DocumentOption)
```

### Route53 레코드 세트 생성 -> images url 연결

### views.py
```python

```

### board / templaes / board / document_list, document_create, document_update, document_detail, document_delete.html
```html

```

```html

```

```html

```

```html

```

```html

```

### board / urls.py
```python
from django.urls import path
from .views import *

app_name = 'board'

urlpatterns = [
    path('', document_list, name='list'),

]
```

### config / urls.py
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('board.urls')),
]
```

### board / forms.py 생성
```python
from django import forms
from .models import Document

class DocumentForm(forms.ModelForm):
    class Meta:
        model = Document
        fields = ['category', 'title', 'text', 'image']
```
