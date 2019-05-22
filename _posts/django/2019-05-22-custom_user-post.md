# Custom User

- models.py
```python
from django.db import models

# Create your models here.
# 기존에 있던 User 모델을 확장해서 내가 원하는 추가 필드를 만드는 것
# 커뮤니티 사이트 - 프로필 사진, 자기소개

from django.contrib.auth.models import User

"""
기존에 모델을 확장(필드 추가)하려면 모델 소스코드를 변경하는데, 모델 만들 때는 models.Model을 상속받음
* 모델은 어떤 모델을 상속받아서 확장할 수도 있다.

기존 User 모델은 AbstractUser 라는 모델을 상속받아서 구현된다.
AbstractUser 모델은 AbstractBaseUser 모델을 상속받아서 구현된다.
* Abstract : 추상, 모델의 설정값에 Abstract 값을 성정할 수 있다.
Abstract 가 설정되어 있으면 실제 모델로 사용 불가

1) 기존 User 모델은 AbstractUser 라는 모델을 상속받아서 필수 필드들이 이미 구현이 되어 있음 - 제일 권장, 프로젝트 시작 시점에 고려, DB 셋팅하기 전에 해야 함(기존에 있는 회원 정보는 유지 안됨)
- 기본 필드들과, 권한 관련 기능 등 장고에서 유저에게 기본적으로 필요한 기능이 구현되어 있음
- 추가 필드가 필요한 경우에는 AbstractUser 모델을 상속 받아 구현하는 것이 제일 편함

2) AbstractBaseUser 로 구현해야 하는 경우
- 모든 필드 구조를 변경할 필요가 있을 때
- 퍼미션 기능 등 장고에서 사용하는 유저 기본기능 전체를 변경하고 싶을 때
- Backend 를 새로 구현해야 함

3) 기본 User 모델을 상속받거나, 확장해서 사용하는 경우 - 기존에 운영중에 확장을 고려해야 할 때 선택 가능, 기존 유저 데이터 유지 + 추가 테이블
* OneToOne 필드
- User + AdditionalInfo(생일, 주소, 주민번호)
-> 회원수정 페이지 AdditionalInfoForm 을 추가로 출력


swappable : AUTH_USER_MODEL 값에 설정된 모델과 교체 가능하다고 미리 설정해 둔 것
class User(AbstractUser):
    class Meta(AbstractUser.Meta):
        swappable = 'AUTH_USER_MODEL'
"""

from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    message = models.TextField(blank=True)
    profile = models.ImageField(upload_to='user_images/profile/%Y/%m/%d', blank=True)

```

- settings.py
```python
AUTH_USER_MODEL = 'account.User'
```

- python manage.py makemigrations
- python manage.py migrate
- python manage.py createsuperuser

- admin.py
```python
from .models import *
from django.contrib.auth.admin import UserAdmin

class CustomUserAdmin(UserAdmin):
    UserAdmin.fieldsets[1][1]['fields']+=('profile','message')
    UserAdmin.add_fieldsets += (
        (('Additional Info'), {'fields':('profile','message')}),
    )

admin.site.register(User, UserAdmin)
```

## email_login 추가

- app 안에 backends.py 생성
```python
from django.contrib.auth.backends import ModelBackend
from django.contrib.auth import get_user_model

class CustomUserBackend(ModelBackend):
    def authenticate(self, request, username=None, password=None, **kwargs):
        user = super().authenticate(request, username, password, **kwargs)

        if user:
            return user

        # id login fail
        # email login
        UserModel = get_user_model()

        if username is None:
            email = kwargs.get(UserModel.EMAIL_FIELD, kwargs.get('email'))
        try:
            user = UserModel._default_manager.get(email=email)

        except UserModel.DoesNotExist:
            UserModel().set_password(password)

        else:
            if user.check_password(password) and self.user_can_authenticate(user):
                return user
```
