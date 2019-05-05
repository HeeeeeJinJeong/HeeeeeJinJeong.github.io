# follow 기능 구현하기

1. accounts / models.py
```python
from django.db import models
from django.contrib.auth.models import User

class Follow(models.Model):
    # 2개 필드 - ForeignKey
    # A가 B를 팔로우 하고 있다.
    # on_delete : 연관된 객체가 삭제된다면 어떻게 할 것이냐?
    # related_name : 참조객체의 입장에서 필드명, 속성값

    # 내가 팔로우 한 사람
    me = models.ForeignKey(User, on_delete=models.CASCADE, related_name='following')

    # 나를 팔로우 한 사람
    you = models.ForeignKey(User, on_delete=models.CASCADE, related_name='follower')

    def __str__(self):
        return self.me.username+" follow "+self.you.username
```

3. python manage.py makemigrations accounts

4. python manage.py migrate accounts 0001

5. views.py
```python
from django.shortcuts import render

# Create your views here.
# 유저 목록이 출력되는 뷰
# + follow 기능
# 중간 테이블을 직접 생성 - 모델

# 유저 모델 -> 유저 모델을 커스터마이징
# 확장하는 방법에 따라
# 1) 새로운 유저 모델을 만드는 방법 - 기존 유저 데이터를 유지할 수 없음
# 2) 기존 모델을 확장하는 방법 - DB 다운 타임 발 alter table - table lock

# 나를 팔로우 - 필드
# 내가 팔로우 - 필드

# 커스터마이징 불가할 경우
# 새로운 모델을 추가

# 사진 모델
# 사진을 좋아요 한 사람 - 필드
# 사진을 저장 한 사람 - 필드

"""
1. 유저 목록 or 유저 프로필에서 팔로우 버튼
1-1. 전체 유저 목록을 출력해주는 뷰 - 유저 모델에 대한 ListView
2. 팔로우 정보를 저장하는 뷰
"""

from .models import Follow

from django.views.generic.list import ListView
from django.contrib.auth.models import User


class UserList(ListView):
    model = User
    template_name = 'accounts/user_list.html'
```

5. urls.py
```python
from django.urls import path
from django.contrib.auth.views import LoginView, LogoutView

from .views import UserList

app_name = 'accounts'

urlpatterns = [
    path('user/list/', UserList.as_view(), name='user_list'),
    path('signin/', LoginView.as_view(template_name='accounts/signin.html'), name='signin'),
    path('signout/', LogoutView.as_view(template_name='accounts/signout.html'), name='signout'),
]
```

6. templates / accounts / user_list.html
```html
{% extends 'base.html' %}

{% block title %}
{% endblock %}

{% block content %}
<div class="row">
    <div class="col"></div>
    <div class="col-6">
        <table class="table">
            <tbody>
            {% for object in object_list %}
                <tr>
                    <td>{{object.username}}</td>
                    <td>{{object.firstname}} {{object.lastname}}</td>
                    <td><a href="" class="btn btn-xs btn-outline-success">Follow</a></td>
                    <td>{{object.follower.all.count}}</td>
                    <td>{{object.following.all.count}}</td>
                </tr>
            {% endfor %}
            </tbody>
        </table>
    </div>
    <div class="col"></div>
</div>
{% endblock %}
```
