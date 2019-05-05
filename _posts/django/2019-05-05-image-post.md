# 이미지 업로드

1. models.py 에 필드 추가
```python
image = models.ImageField(upload_to='timeline_photo/%Y/%m/%d')
# upload_to : 함수를 사용해서 폴더를 동적으로 설정할 수 있다.
```

2. python manage.py makemigrations photo

3. python manage.py migrate photo

4. config / settings.py
맨 아래에 내용 추가
```python
STATIC_URL = '/static/'

MEDIA_URL = '/res/'
MEDIA_ROOT = os.path.join(BASE_DIR,'media')
```
