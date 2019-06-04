# Tagging

pip install django-tagging
```python
from tagging.fields import TagField
class Post(models.Model):
    category = models.ForeignKey(Category, on_delete=models.SET_NULL, blank=True, null=True)
    title = models.CharField(max_length=100)
    text = RichTextUploadingField()
    slug = models.SlugField(max_length=120, unique=True, allow_unicode=True, db_index=True)
    material = models.FileField(upload_to='material/%Y/%m/%d', blank=True)
    tag = TagField()
```

- setting
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'debug_toolbar',
    'django_extensions',
    'ckeditor',
    'ckeditor_uploader',

    'post',
    'tagging',
]
```
