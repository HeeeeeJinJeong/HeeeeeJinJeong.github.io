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


### app : python manage.py startapp
- board

### model
- Document - author, category, title, slug, text, image, created, updated
- Category - slug, name


