# 배포 Deploy : 서버에 올린다.
## Pythonanywhere /w Github
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
