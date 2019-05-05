# Login 기능 구현

1. accounts 앱 생성
2. config / settings.py 수정
- INSTALLED APP에 앱 추가
```python
INSTALLED_APPS = [
    'accounts',
]
# 맨 밑에 아래 내용 추가
# 로그인 후 profile 페이지 대신 이동할 페이지 주소 설정
LOGIN_REDIRECT_URL = '/'

# 로그인 페이지의 url 알려주기
from django.urls import reverse_lazy
LOGIN_URL = reverse_lazy('accounts:signin')
```

3. urls.py 생성 후 아래 내용 작성
```python
from django.urls import path
from django.contrib.auth.views import LoginView, LogoutView

app_name = 'accounts'

urlpatterns = [
    path('signin/', LoginView.as_view(template_name='accounts/signin.html'), name='signin'),
    path('signout/', LogoutView.as_view(template_name='accounts/signout.html'), name='signout'),
]
```

4. 템플릿 만들기
- signin
```html
{% extends 'base.html' %}

{% block title %}
{% endblock %}

{% block content %}
<div class="row">
    <div class="col"></div>
    <div class="col-6">
        <div class="alert alert-primary">Please enter your login information.</div>
        <form action="" method="post">
            {% csrf_token %}
            {{form.as_p}}
            <input type="submit" value="Sign in" class="btn btn-outline-primary">
        </form>
    </div>
    <div class="col"></div>
</div>
{% endblock %}
```

- signout
```html
{% extends 'base.html' %}

{% block title %}
{% endblock %}

{% block content %}
<div class="row">
    <div class="col"></div>
    <div class="col-6">
        <div class="alert alert-primary">You successfully sign out.</div>
        <a href="{% url 'accounts:signin' %}" class="btn btn-outline-success">Sign In</a>
    </div>
    <div class="col"></div>
</div>
{% endblock %}
```

