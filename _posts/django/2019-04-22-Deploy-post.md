# 배포 Deploy : 서버에 올린다.
## 1. Pythonanywhere /w Github

https://github.com/HeeeeeJinJeong/HeeeeeJinJeong.github.io/blob/master/_posts/django/PythonAnywhere%20Deploy.pptx


## 2. Heroku
1. macOS : brew tap heroku/brew && brew install heroku 설치
2. 파이참 재실행
3. 터미널에서 heroku
```terminal
(venv) ➜  dstargram_project git:(master) ✗ heroku
CLI to interact with Heroku

VERSION
  heroku/7.24.1 darwin-x64 node-v11.14.0

USAGE
  $ heroku [COMMAND]

COMMANDS
  access          manage user access to apps
  addons          tools and services for developing, extending, and operating your app
  apps            manage apps on Heroku
  auth            check 2fa status
  authorizations  OAuth authorizations
  autocomplete    display autocomplete installation instructions
  buildpacks      scripts used to compile apps
  certs           a topic for the ssl plugin
  ci              run an application test suite on Heroku
  clients         OAuth clients on the platform
  config          environment variables of apps
  container       Use containers to build and deploy Heroku apps
  domains         custom domains for apps
  drains          forward logs to syslog or HTTPS
  features        add/remove app features
  git             manage local git repository for app
  help            display help for heroku
  keys            add/remove account ssh keys
  labs            add/remove experimental features
  local           run heroku app locally
  logs            display recent log output
  maintenance     enable/disable access to app
  members         manage organization members
  notifications   display notifications
  orgs            manage organizations
  pg              manage postgresql databases
  pipelines       groups of apps that share the same codebase
  plugins         list installed plugins
  ps              Client tools for Heroku Exec
  psql            open a psql shell to the database
  redis           manage heroku redis instances
  regions         list available regions for deployment
  releases        display the releases for an app
  reviewapps      disposable apps built on GitHub pull requests
  run             run a one-off process inside a Heroku dyno
  sessions        OAuth sessions
  spaces          manage heroku private spaces
  status          status of the Heroku platform
  teams           manage teams
  update          update the Heroku CLI
  webhooks        setup HTTP notifications of app activity

(venv) ➜  dstargram_project git:(master) ✗ 
```

4. 추가 모듈 설치
- pip install dj-database-url : 데이터 베이스 관련 옵션을 변수로 쉽게 접근할 수 있기 해주는 유틸리티
- pip install gunicorn : wsgi용 미들웨어 - 웹서버와 장고 사이의 다리 역할
- pip install whitenoise : static 파일 서빙용 미들웨어
- pip install psycopg2-binary : postgreSQL용 드라이버
```terminal
(venv) ➜  dstargram_project git:(master) pip install dj-database-url
Collecting dj-database-url
  Downloading https://files.pythonhosted.org/packages/d4/a6/4b8578c1848690d0c307c7c0596af2077536c9ef2a04d42b00fabaa7e49d/dj_database_url-0.5.0-py2.py3-none-any.whl
Installing collected packages: dj-database-url
Successfully installed dj-database-url-0.5.0
(venv) ➜  dstargram_project git:(master) ✗ pip install gunicorn
Collecting gunicorn
  Downloading https://files.pythonhosted.org/packages/8c/da/b8dd8deb741bff556db53902d4706774c8e1e67265f69528c14c003644e6/gunicorn-19.9.0-py2.py3-none-any.whl (112kB)
    100% |████████████████████████████████| 122kB 1.5MB/s 
Installing collected packages: gunicorn
Successfully installed gunicorn-19.9.0
(venv) ➜  dstargram_project git:(master) ✗ pip install whitenoise
Collecting whitenoise
  Downloading https://files.pythonhosted.org/packages/fd/2a/b51377ab9826f0551da19951257d2434f46329cd6cfdf9592ea9ca5f6034/whitenoise-4.1.2-py2.py3-none-any.whl
Installing collected packages: whitenoise
Successfully installed whitenoise-4.1.2
(venv) ➜  dstargram_project git:(master) ✗ pip install psycopg2-binary
Collecting psycopg2-binary
  Downloading https://files.pythonhosted.org/packages/f1/88/549d331f884c71dfbcdca413c6fa60bc30719cf533b05b2b05eea6c91afb/psycopg2_binary-2.8.2-cp37-cp37m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (1.4MB)
    100% |████████████████████████████████| 1.4MB 3.9MB/s 
Installing collected packages: psycopg2-binary
Successfully installed psycopg2-binary-2.8.2
```

5. requirements.txt 만들기
- pip freeze > requirements.txt

6. 모듈 설정 - settings.py
```python
DEBUG = False

ALLOWED_HOSTS = ['*']

MIDDLEWARE = [ # 아래에 whitenoise 추가
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
]

# DATABASES 밑에 내용 추가
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}

import dj_database_url
DATABASES['default'].update(dj_database_url.config(conn_max_age=500))

# 맨 아래
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles') # static 파일들을 모아줌
```
- 루트 파일 밑에 'Procfile' 파일 생성 후 아래 내용 입력
```text
web: gunicorn config.wsgi
```
- 'runtime.txt' 파일 생성
```text
python-3.7.0
```
- '.gitignore'
```text
*.pyc
*~
/venv
__pycache__
db.sqlite3
.DS_Store
```

7. git - heroku login
```terminal
(venv) ➜  dstargram_project git:(master) ✗ heroku login
heroku: Press any key to open up the browser to login or q to exit: 

Opening browser to https://cli-auth.heroku.com/auth/browser/329bb7bc-faa1-4752-8291-4fec4c09acd7
Logging in... done
Logged in as misakiyoshikuni@gmail.com


(venv) ➜  dstargram_project git:(master) ✗ git init
Reinitialized existing Git repository in /Users/heejin/PycharmProjects/dstargram_project/.git/
(venv) ➜  dstargram_project git:(master) ✗ git add -A .
(venv) ➜  dstargram_project git:(master) ✗ git commit -m "heroku commit"
[master 7014d45] heroku commit
 6 files changed, 80 insertions(+), 60 deletions(-)
 create mode 100644 .gitignore
 create mode 100644 Procfile
 create mode 100644 runtime.txt
```

8. heroku app 만들기
```terminal
(venv) ➜  dstargram_project git:(master) ✗ heroku create wps-dstargram
Creating ⬢ wps-dstargram... done
https://wps-dstargram.herokuapp.com/ | https://git.heroku.com/wps-dstargram.git
(venv) ➜  dstargram_project git:(master) ✗ 
```

9. 업로드
```terminal
(venv) ➜  dstargram_project git:(master) ✗ git push heroku master
Enumerating objects: 8145, done.
Counting objects: 100% (8145/8145), done.
Delta compression using up to 12 threads
Compressing objects: 100% (5056/5056), done.
Writing objects: 100% (8145/8145), 33.33 MiB | 744.00 KiB/s, done.
Total 8145 (delta 2085), reused 7848 (delta 2043)
remote: Compressing source files... done.
remote: Building source:
remote: 
remote: -----> Python app detected
remote:  !     Python has released a security update! Please consider upgrading to python-3.7.3
remote:        Learn More: https://devcenter.heroku.com/articles/python-runtimes
remote: -----> Installing python-3.7.0
remote: -----> Installing pip
remote: -----> Installing SQLite3
remote: -----> Installing requirements with pip
remote:        Collecting dj-database-url==0.5.0 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 1))
remote:          Downloading https://files.pythonhosted.org/packages/d4/a6/4b8578c1848690d0c307c7c0596af2077536c9ef2a04d42b00fabaa7e49d/dj_database_url-0.5.0-py2.py3-none-any.whl
remote:        Collecting Django==2.2 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 2))
remote:          Downloading https://files.pythonhosted.org/packages/54/85/0bef63668fb170888c1a2970ec897d4528d6072f32dee27653381a332642/Django-2.2-py3-none-any.whl (7.4MB)
remote:        Collecting django-disqus==0.5 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 3))
remote:          Downloading https://files.pythonhosted.org/packages/7d/d6/fc7da50586901cbbf25062f1912095c37faa2f28c2bd8d5831794e42412d/django-disqus-0.5.tar.gz
remote:        Collecting gunicorn==19.9.0 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 4))
remote:          Downloading https://files.pythonhosted.org/packages/8c/da/b8dd8deb741bff556db53902d4706774c8e1e67265f69528c14c003644e6/gunicorn-19.9.0-py2.py3-none-any.whl (112kB)
remote:        Collecting mock==2.0.0 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 5))
remote:          Downloading https://files.pythonhosted.org/packages/e6/35/f187bdf23be87092bd0f1200d43d23076cee4d0dec109f195173fd3ebc79/mock-2.0.0-py2.py3-none-any.whl (56kB)
remote:        Collecting pbr==5.2.0 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 6))
remote:          Downloading https://files.pythonhosted.org/packages/07/3e/22d1d35a4b51706ca3590c54359aeb5fa7ea60df46180143a3ea13d45f29/pbr-5.2.0-py2.py3-none-any.whl (107kB)
remote:        Collecting Pillow==6.0.0 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 7))
remote:          Downloading https://files.pythonhosted.org/packages/c1/e6/ce127fa0ac17775bc7887c432ffe945c49ae141f01b477b7cd5e63b16bb5/Pillow-6.0.0-cp37-cp37m-manylinux1_x86_64.whl (2.0MB)
remote:        Collecting psycopg2-binary==2.8.2 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 8))
remote:          Downloading https://files.pythonhosted.org/packages/a5/bf/870a127de76b5b01c26eb8056f42a315eb9cb625b87cdee896c71bf73ca1/psycopg2_binary-2.8.2-cp37-cp37m-manylinux1_x86_64.whl (2.9MB)
remote:        Collecting pytz==2019.1 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 9))
remote:          Downloading https://files.pythonhosted.org/packages/3d/73/fe30c2daaaa0713420d0382b16fbb761409f532c56bdcc514bf7b6262bb6/pytz-2019.1-py2.py3-none-any.whl (510kB)
remote:        Collecting six==1.12.0 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 10))
remote:          Downloading https://files.pythonhosted.org/packages/73/fb/00a976f728d0d1fecfe898238ce23f502a721c0ac0ecfedb80e0d88c64e9/six-1.12.0-py2.py3-none-any.whl
remote:        Collecting sqlparse==0.3.0 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 11))
remote:          Downloading https://files.pythonhosted.org/packages/ef/53/900f7d2a54557c6a37886585a91336520e5539e3ae2423ff1102daf4f3a7/sqlparse-0.3.0-py2.py3-none-any.whl
remote:        Collecting whitenoise==4.1.2 (from -r /tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/requirements.txt (line 12))
remote:          Downloading https://files.pythonhosted.org/packages/fd/2a/b51377ab9826f0551da19951257d2434f46329cd6cfdf9592ea9ca5f6034/whitenoise-4.1.2-py2.py3-none-any.whl
remote:        Installing collected packages: dj-database-url, pytz, sqlparse, Django, six, pbr, mock, django-disqus, gunicorn, Pillow, psycopg2-binary, whitenoise
remote:          Running setup.py install for django-disqus: started
remote:            Running setup.py install for django-disqus: finished with status 'done'
remote:        Successfully installed Django-2.2 Pillow-6.0.0 dj-database-url-0.5.0 django-disqus-0.5 gunicorn-19.9.0 mock-2.0.0 pbr-5.2.0 psycopg2-binary-2.8.2 pytz-2019.1 six-1.12.0 sqlparse-0.3.0 whitenoise-4.1.2
remote: 
remote: -----> $ python manage.py collectstatic --noinput
remote:        120 static files copied to '/tmp/build_7d3d2d17ad91884722b7ea16ae0704a9/staticfiles'.
remote: 
remote: -----> Discovering process types
remote:        Procfile declares types -> web
remote: 
remote: -----> Compressing...
remote:        Done: 98.8M
remote: -----> Launching...
remote:        Released v5
remote:        https://wps-dstargram.herokuapp.com/ deployed to Heroku
remote: 
remote: Verifying deploy... done.
To https://git.heroku.com/wps-dstargram.git
 * [new branch]      master -> master
(venv) ➜  dstargram_project git:(master) ✗ 
```

10. DB 초기화
```terminal
(venv) ➜  dstargram_project git:(master) ✗ heroku run python manage.py migrate
Running python manage.py migrate on ⬢ wps-dstargram... up, run.5327 (Free)
Operations to perform:
  Apply all migrations: accounts, admin, auth, contenttypes, photo, sessions, sites
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying accounts.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying photo.0001_initial... OK
  Applying photo.0002_auto_20190501_0504... OK
  Applying photo.0003_photo_like... OK
  Applying photo.0004_photo_save_post... OK
  Applying photo.0005_auto_20190502_0817... OK
  Applying photo.0006_auto_20190503_0109... OK
  Applying photo.0007_auto_20190503_0405... OK
  Applying sessions.0001_initial... OK
  Applying sites.0001_initial... OK
  Applying sites.0002_alter_domain_unique... OK
(venv) ➜  dstargram_project git:(master) ✗ 
```

11. 관리자 비번 만들기
```terminal
(venv) ➜  dstargram_project git:(master) ✗ heroku run python manage.py createsuperuser
Running python manage.py createsuperuser on ⬢ wps-dstargram... up, run.6433 (Free)
Username (leave blank to use 'u55301'): admin
Email address: 
Password: 
Password (again): 
This password is too short. It must contain at least 8 characters.
Bypass password validation and create user anyway? [y/N]: y
Superuser created successfully.
(venv) ➜  dstargram_project git:(master) ✗ heroku open
```

12. app 접속
- https://wps-dstargram.herokuapp.com/

### S3 셋팅 - 이미지 띄우기(아마존)
- boto3 : 아마존S3 를 사용할 수 있게 pip install boto3
- django-storages : 장고 프로젝트에서 특정 storages 를 사용할 수 있게 pip install django-storages

1. static 파일들 대체
```python
# STATIC_URL = '/static/'
# STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

AWS_ACCESS_KEY_ID = 'your-key'
AWS_SECRET_ACCESS_KEY = 'your-secret-key'
AWS_REGION = 'ap-northeast-2'
AWS_STORAGE_BUCKET_NAME = 'your-bucket-name'
AWS_S3_CUSTOM_DOMAIN = '%s.s3.%s.amazonaws.com' % (AWS_STORAGE_BUCKET_NAME,AWS_REGION)
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
AWS_DEFAULT_ACL = 'public-read'
AWS_LOCATION = 'static'

STATIC_URL = 'https://%s/%s/' % (AWS_S3_CUSTOM_DOMAIN, AWS_LOCATION)
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```

- python manage.py collectstatic
```terminal
(venv) ➜  dstargram_project git:(master) ✗ python manage.py collectstatic

You have requested to collect static files at the destination
location as specified in your settings.

This will overwrite existing files!
Are you sure you want to do this?

Type 'yes' to continue, or 'no' to cancel: yes

120 static files copied.
(venv) ➜  dstargram_project git:(master) ✗ 

```

2. media 파일들 대체
- INSTALLED_APPS 에 storages 추가
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'accounts',
    'photo',
    'disqus',
    'django.contrib.sites',
    'storages',
]
```

```python
# MEDIA_URL = '/res/'
# MEDIA_ROOT = os.path.join(BASE_DIR,'media')

DEFAULT_FILE_STORAGE = 'config.asset_storage.MediaStorage'
```

- config / asset_storage.py 파일 생성
```python
from storages.backends.s3boto3 import S3Boto3Storage

class MediaStorage(S3Boto3Storage):
    location = 'media'
    file_overwrite = False
```

- config / urls.py 의 아래 내용 지우거나 주석처리
```python
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

- 이미 업로드 한 이미지 파일 처리, 루트 밑에 s3_upload 파일 생성(한번 실행 후 런서버)
```python
import boto3 # s3에 파일을 업로드
import os # 현재 업로드 할 파일 탐색

def upload_files(search_path, target_path):
    session = boto3.Session(
        aws_access_key_id='your-key',
        aws_secret_access_key='your-secret-key',
        region_name='ap-northeast-2'
    )

    s3 = session.resource('s3')
    bucket = s3.Bucket('your-bucket-name')

    for current_dir, sub_dirs, files in os.walk(search_path):
        print(current_dir, sub_dirs, files)

        for file in files:
            full_path = os.path.join(current_dir, file)
            # print(full_path)

            with open(full_path, 'rb') as data:
                bucket.put_object(Key=target_path+'/'+(full_path.replace("\\", "/"))[len(search_path)+1:],Body=data, ACL='public-read') # s3 경로


if __name__=="__main__":
    upload_files('./media', 'media')
```

3. AWS
- Route 53 -> DNS관리 -> 호스팅 영역 생성 -> 레코드 세트 생성

4. RDS -> 파라미터 그룹 -> 파라미터 그룹 생성 (한국어 지원을 위해) -> postgreg9.6

5. settings.py
```python
# pip install psycopg2-binary
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME':'dstargram',
        'USER':'',
        'PASSWORD':'',
        'HOST':'dstargram.czf1yineqjau.us-east-2.rds.amazonaws.com',
        'PORT':'5432',
    }
}
```
