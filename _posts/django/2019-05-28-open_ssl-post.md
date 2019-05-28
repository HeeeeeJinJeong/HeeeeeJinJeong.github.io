# Openssl 로 인증서 만들기

### openssl version 으로 버전 확인하기
```terminal
➜  ~ git:(master) ✗ openssl version
```

### 키 파일 생성
```terminal
➜  ~ git:(master) ✗ openssl genrsa 1024 > django.key
```

### cert 파일 생성
```terminal
➜  ~ git:(master) ✗ openssl req -new -x509 -nodes -sha256 -days 365 -key django.key > django.cert
```

### django.key 와 django.cert 파일을 해당 프로젝트 폴더에 복사

### pip install django-sslserver

### settings.py INSTALLED_APPS 에 'sslserver' 추가

### 명령어 실행 (둘 중 어느걸로 해도 상관 없음)
```python
python manage.py runsslserver --certificate django.cert --dey django.key
python manage.py runsslserver
```
