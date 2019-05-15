# 소셜 로그인 (Auth)

1. pip install django-allauth

2. settings.py
```python
# 로그인 시도할 때 어떤 로직을 사용할것인지
# 통합 User Model
# 고객, 판매자, 관리자 -> User Model -> Group
AUTHENTICATION_BACKENDS = (
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
)
```

3. settings.py / INSTALLED_APPS
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'board',
    
    # 아래내용 추가
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.naver', # naver
]
```

4. settings.py
```python
SITE_ID = 1
```

5. config / urls.py
```python
path('accounts/', include('allauth.urls'))
```

6. python manage.py migrate

------------------------------------------------------

### naver 에 허락맡기

1. https://developers.naver.com/appinfo 접속
2. 애플리케이션 등록
3. python manage.py runserver
4. 관리자 페이지 설정
