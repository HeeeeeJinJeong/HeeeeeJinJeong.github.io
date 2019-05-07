# 회원가입 구현하기

- 기본에 입력받는 뷰는 CreateView를 상속 -> 커스텀이 힘듦
- 회원가입 -> User 모델에 값을 입력받는다. -> CreateView
- 회원가입시 모델 필드 외에 추가 입력이 필요하다.
- 커스텀은 함수형 뷰가 적절

1. accounts / views.py
```python
# class based view -> dispatch -> get, post
def signup(request):
    # class based view -> dispatch -> get, post
    if request.method == "POST":
        # Todo : 입력받은 내용을 이용해서 회원 객체 생성
        # 입력받은 내용 확인하기
        username = request.POST.get('username')
        password = request.POST.get('password')
        password2 = request.POST.get('password2')
        print(username,password,password2) # test 1234 1234 -> 터미널에서만 확인할 수 있음

        # 회원 객체 생성하기 from django.contrib.auth.models import User
        # -> 이제 관리자계정에서 확인할 수 있음
        user = User()
        user.username = username
        # user.password = password # 암호화가 안됨
        user.set_password(password) # 암호화
        user.save()
        return render(request, 'accounts/signup_complete.html')
    else:
        # Todo : form 객체를 만들어서 전달
        # form 에는 username, password 만 입력 받으면 됨
        context_values = {'form':'This is form'}
        return render(request, 'accounts/signup.html', context_values)
        
# render
# 1. 템플릿 불러오기
# 2. 템플릿 랜더링하기
# 3. HTTP Response 하기
```

- 모델폼을 이용해서 코드를 간결하게 바꿈, forms.py 생성
```python
from django.contrib.auth.models import User
from django import forms

class SignUpForm(forms.ModelForm):
    # 작성 중 비밀번호 안보이게 하기, Meta보다 우선순위가 높음
    password = forms.CharField(label='Password', widget=forms.PasswordInput) # 우선순위가 높음
    password2 = forms.CharField(label='Repeat Password', widget=forms.PasswordInput)

    class Meta:
        model = User
        # fields 에는 해당 모델에 대해 입력받을 필드들을 나열한다.
        # 추가 필드도 포함될 수 있다. -> 필드 목록과 추가 필드가 겹치면 오버라이드 됨
        # fields 에 작성한 순서대로 출력한다.
        fields = ['username', 'password', 'password2', 'first_name', 'last_name', 'email'] # 입력 받을 내용
        # fields = '__all__' # 전부 입력 받을 때는 __all__

        # Todo : 필드의 기본 값, Placeholder 설정법, css Class 설정법, Validator 설정법, help text 설정법
        # Todo : 커스텀 필드 만드는 법
```

- views.py 수정
```python
from django.contrib.auth.models import User
from .forms import SignUpForm

def signup(request):
    # class based view -> dispatch -> get, post
    if request.method == "POST":
        # Todo : 입력받은 내용을 이용해서 회원 객체 생성
        # 입력받은 내용 확인하기
        # 모델폼을 이용해서 코드를 간결하게 바꿈
        username = request.POST.get('username')
        password = request.POST.get('password')
        first_name = request.POST.get('first_name')
        last_name = request.POST.get('last_name')
        email = request.POST.get('email')


        # 회원 객체 생성하기 from django.contrib.auth.models import User
        # -> 이제 관리자계정에서 확인할 수 있음
        user = User()
        user.username = username
        user.set_password(password) # 암호화
        user.first_name = first_name
        user.last_name = last_name
        user.email = email

        user.save()
        return render(request, 'accounts/signup_complete.html')
    else:
        signup_form = SignUpForm()
        context_values = {'form':signup_form}
        return render(request, 'accounts/signup.html', context_values)
```

## 코드 간결하게
- views.py
```python
from .forms import SignUpForm

def signup(request):
    # class based view -> dispatch -> get, post
    if request.method == "POST":
        # form 이 채워져 있는 형태
        signup_form = SignUpForm(request.POST)

        # form validation
        if signup_form.is_valid():
            # 1. 저장하고 인스턴스 생성
            user_instance = signup_form.save(commit=False) # commit=False : DB 에는 저장 안됐으나 인스턴스는 생성
            # 2. 패스워드 암호화 -> 저장
            # form 이 가지고 있는 cleaned_data : 유효한 문자만 남긴 상태로, 처리 과정을 거친 데이터
            user_instance.set_password(signup_form.cleaned_data['password'])
            user_instance.save()

            return render(request, 'accounts/signup_complete.html', {'username':user_instance.username})
    else:
        signup_form = SignUpForm()

    return render(request, 'accounts/signup.html', {'form':signup_form})
```

- forms.py
```python
from django.contrib.auth.models import User
from django import forms

class SignUpForm(forms.ModelForm):
    # 작성 중 비밀번호 안보이게 하기, Meta보다 우선순위가 높음
    password = forms.CharField(label='Password', widget=forms.PasswordInput) # 우선순위가 높음
    password2 = forms.CharField(label='Repeat Password', widget=forms.PasswordInput)

    class Meta:
        model = User
        # fields 에는 해당 모델에 대해 입력받을 필드들을 나열한다.
        # 추가 필드도 포함될 수 있다. -> 필드 목록과 추가 필드가 겹치면 오버라이드 됨
        # fields 에 작성한 순서대로 출력한다.
        fields = ['username', 'password', 'password2', 'first_name', 'last_name', 'email'] # 입력 받을 내용
        # fields = '__all__' # 전부 입력 받을 때는 __all__

    # Repeat Password가 일치하지 않을 때
    # def clean_필드명(self):
    def clean_password2(self):
        cd = self.cleaned_data

        if cd['password'] != cd['password2']:
            raise forms.ValidationError('비밀번호가 일치하지 않습니다.')
        # 항상 필드의 값을 리턴한다.
        return cd['password2']
```


2. accounts / urls.py
```python
path('signup/', signup, name='signup')
```


3. template
- signup.html
```html
{% extends 'base.html' %}

{% block title %}
{% endblock %}

{% block content %}
<div class="row">
    <div class="col"></div>
    <div class="col-6">
    
        <div class="alert alert-primary">Please enter your accounts information.</div>
        
        <form action="" method="post">
            {% csrf_token %}
            
            {{form.as_p}}
            <input type="submit" value="Sign up" class="btn btn-outline-primary">
        </form>
    </div>
    <div class="col"></div>
</div>
{% endblock %}
```

- signup_complete.html
```html
{% extends 'base.html' %}

{% block title %}
{% endblock %}

{% block content %}
<div class="row">
    <div class="col"></div>
    <div class="col-6">
    
        <div class="alert alert-primary">{{username}} You successfully signed up</div>

        <a href="{% url 'accounts:signin' %}" class="btn btn-outline-success">Move to Sign In</a>

    </div>
    <div class="col"></div>
</div>
{% endblock %}
```
