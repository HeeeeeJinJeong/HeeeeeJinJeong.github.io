# signal : save 오버라이드 대신 사용
- 글 삭제시 이미지도 같이 삭제 구현
- signal 종류
1. pre_save : 저장하기 전
2. post_save : 저장한 후
3. pre_delete : 삭제 전
4. post_delete : 삭제 후

### 오버라이드 대신 사용할 수 있음
- Hook
- Signal
- Injection

### signal 받는 법
1. 어떤 시그널이 발생했을 때 반응할 것인지?
2. 해당 시그널이 발생한 여부 확인

- python manage.py shell 에서 경로 확인
```terminal
Python 3.7.1 (v3.7.1:260ec2c36a, Oct 20 2018, 03:13:28) 
[Clang 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from photo.models import Photo
>>> Photo.objects.all()
<QuerySet [<Photo: Photo object (2)>, <Photo: Photo object (1)>]>
>>> p1 = Photo.objects.get(pk=2)
>>> p1.image
<ImageFieldFile: timeline_photo/2019/05/09/KakaoTalk_Photo_2019-05-01-10-55-06.jpeg>
>>> str(p1.image)
'timeline_photo/2019/05/09/KakaoTalk_Photo_2019-05-01-10-55-06.jpeg'
>>> 

```

- views.py
```python
# signal
# 1. 어떤 시그널이 발생했을 때 반응할 것인지?
# 2. 해당 시그널이 발생한 여부 확인

from django.db.models.signals import post_delete
from django.dispatch import receiver

import boto3
from django.conf import settings


@receiver(post_delete, sender=Photo) # 어떤 시그널이 발생헀는지, 누가 발생시켰는지
def post_delete(sender, instance, **kwargs):
    storage = instance.image.storage

    if storage.exists(str(instance.image)):
        storage.delete(str(instance.image))

    # session = boto3.Session(
    #     aws_access_key_id=settings.AWS_ACCESS_KEY_ID,
    #     aws_secret_access_key=settings.AWS_SECRET_ACCESS_KEY,
    #     region_name=settings.AWS_REGION,
    # )
    #
    # s3 = session.resource('s3')
    #
    # # s3 Object : s3 에 업로드 된 파일 객체를 얻어도는 클래스
    # # arg1 : 버킷네임
    # # arg2 : 파일경로 - key
    # image = s3.Object(settings.AWS_STORAGE_BUCKET_NAME, "media/"+str(instance.image)) # 버킷 네임, 파일경로
    # image.delete()
```
