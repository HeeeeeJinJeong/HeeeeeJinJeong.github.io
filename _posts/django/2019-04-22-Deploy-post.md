# 배포 Deploy : 서버에 올린다.
## 1. Pythonanywhere /w Github
- github에 소스코드 업로드, requirements 도 같이
- Pythonanywhere 가입



1. pip freeze > requirements.txt
2. git init
3. git add -A .
4. git commit -m "First commit"
5. git remote add origin [git 주소]
6. git push -u origin master

```terminal
(venv) ➜  bookmark_project git:(master) ✗ pwd
/Users/heejin/PycharmProjects/bookmark_project
(venv) ➜  bookmark_project git:(master) ✗ git init
Initialized empty Git repository in /Users/heejin/PycharmProjects/bookmark_project/.git/
(venv) ➜  bookmark_project git:(master) ✗ git add -A .
(venv) ➜  bookmark_project git:(master) ✗ git commit -m "First commit"
[master (root-commit) 71dbb73] First commit
 27 files changed, 1055 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 .idea/bookmark_project.iml
 create mode 100644 .idea/misc.xml
 create mode 100644 .idea/modules.xml
 create mode 100644 .idea/workspace.xml
 create mode 100644 bookmark/__init__.py
 create mode 100644 bookmark/admin.py
 create mode 100644 bookmark/apps.py
 create mode 100644 bookmark/migrations/0001_initial.py
 create mode 100644 bookmark/migrations/0002_auto_20190429_0151.py
 create mode 100644 bookmark/migrations/0003_bookmark_contents.py
 create mode 100644 bookmark/migrations/__init__.py
 create mode 100644 bookmark/models.py
 create mode 100644 bookmark/templates/bookmark/bookmark_create.html
 create mode 100644 bookmark/templates/bookmark/bookmark_delete.html
 create mode 100644 bookmark/templates/bookmark/bookmark_detail.html
 create mode 100644 bookmark/templates/bookmark/bookmark_list.html
 create mode 100644 bookmark/templates/bookmark/bookmark_update.html
 create mode 100644 bookmark/tests.py
 create mode 100644 bookmark/urls.py
 create mode 100644 bookmark/views.py
 create mode 100644 config/__init__.py
 create mode 100644 config/settings.py
 create mode 100644 config/urls.py
 create mode 100644 config/wsgi.py
 create mode 100644 layout/base.html
 create mode 100755 manage.py
(venv) ➜  bookmark_project git:(master) git remote add origin https://github.com/HeeeeeJinJeong/wps_bookmark.git
fatal: remote origin already exists.
(venv) ➜  bookmark_project git:(master) git push -u origin master
Enumerating objects: 43, done.
Counting objects: 100% (43/43), done.
Delta compression using up to 12 threads
Compressing objects: 100% (39/39), done.
Writing objects: 100% (43/43), 13.86 KiB | 2.77 MiB/s, done.
Total 43 (delta 9), reused 0 (delta 0)
remote: Resolving deltas: 100% (9/9), done.
To https://github.com/HeeeeeJinJeong/wps_bookmark.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
(venv) ➜  bookmark_project git:(master) 
```

- consoles
```terminal
04:53 ~/bookmark (master)$ virtualenv --python=python3.7 venv
Running virtualenv with interpreter /usr/bin/python3.7
Using base prefix '/usr'
/usr/local/lib/python2.7/dist-packages/virtualenv.py:1041: DeprecationWarning: the imp module is deprecated in favour of importlib; see the module's d
ocumentation for alternative uses
  import imp
New python executable in /home/heejinnnn/bookmark/venv/bin/python3.7
Also creating executable in /home/heejinnnn/bookmark/venv/bin/python
Installing setuptools, pip, wheel...done.
04:54 ~/bookmark (master)$ source venv/bin/activate
(venv) 04:55 ~/bookmark (master)$ pip install -r requirements.txt
Looking in links: /usr/share/pip-wheels
Collecting Django==2.2 (from -r requirements.txt (line 1))
  Downloading https://files.pythonhosted.org/packages/54/85/0bef63668fb170888c1a2970ec897d4528d6072f32dee27653381a3326
42/Django-2.2-py3-none-any.whl (7.4MB)
     |████████████████████████████████| 7.5MB 26.4MB/s 
Collecting pytz==2019.1 (from -r requirements.txt (line 2))
  Downloading https://files.pythonhosted.org/packages/3d/73/fe30c2daaaa0713420d0382b16fbb761409f532c56bdcc514bf7b6262b
b6/pytz-2019.1-py2.py3-none-any.whl (510kB)
     |████████████████████████████████| 512kB 30.1MB/s 
Collecting sqlparse==0.3.0 (from -r requirements.txt (line 3))
  Downloading https://files.pythonhosted.org/packages/ef/53/900f7d2a54557c6a37886585a91336520e5539e3ae2423ff1102daf4f3
a7/sqlparse-0.3.0-py2.py3-none-any.whl
Installing collected packages: pytz, sqlparse, Django
Successfully installed Django-2.2 pytz-2019.1 sqlparse-0.3.0
```
```terminal
(venv) ➜  bookmark_project git:(master) ✗ git add .
(venv) ➜  bookmark_project git:(master) ✗ git commit -m "commit"
[master febee44] commit
 2 files changed, 44 insertions(+), 34 deletions(-)
(venv) ➜  bookmark_project git:(master) git push
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 12 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 939 bytes | 939.00 KiB/s, done.
Total 6 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), completed with 5 local objects.
To https://github.com/HeeeeeJinJeong/wps_bookmark.git
   21f9111..febee44  master -> master
(venv) ➜  bookmark_project git:(master) ✗
```

————————
git pull
————————

- wsgi
```terminal
import os
import sys
path = '/home/heejinnnn/wps_bookmark'
if path not in sys.path:
    sys.path.append(path)

from django.core.wsgi import get_wsgi_application
from django.contrib.staticfiles.handlers import StaticFilesHandler

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')

application = StaticFilesHandler(get_wsgi_application())
```
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
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
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

7. git
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
(venv) ➜  dstargram_project git:(master) ✗ 
```

12. app 접속
https://wps-dstargram.herokuapp.com/
