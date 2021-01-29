## django 数据库使用教程
### 知识点

#### django 数据库相关开发流程
1. 修改 model(models.py);
2. `python manage.py makemigrations $app` 生成对实体类的变动后待执行脚本;
3. `python manage.py migrate` 命令将 migrations 下的变动执行到数据库；

tips:
a. $project/settings.py INSTALLED_APPS 中配置要使用的应用;
b. 可在 $apps/migrations 文件夹下看到，migrate 命令将要执行的脚本,sqlmigrate 命令可看到实际将要执行的 sql 语句，例 `python manage.py sqlmigrate polls 0001`，实际未执行 sql;

#### django 自带管理后台
1. `python manage.py createsuperuser` 创建 admin 下用户;
2. 使 $app 中的 model 在 admin界面中可管理，修改 $app/admin.py 文件;
``` python
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```

#### django 数据库连接外部配置化 tips
删除 settings.py 文件，项目目录结构修改为
```
.
└── settings/
    ├── __init__.py  <= 不放入版本控制系统
    ├── common.py
    ├── dev.py
    └── prod.py
```
`common.py` 文件中为 `settings.py` 中原内容，在`prod.py`中导入common中的所有内容，然后配置想要覆盖的配置项。
```python
from __future__ import absolute_import # optional, but I like it
from .common import *

# Production overrides
DEBUG = False
#...
```
`__init__.py` 中配置具体要启用的配置文件。
```py
from __future__ import absolute_import
# set environment
from .dev import *  

# DJANGO SECRETS
DATABASES['default']['NAME'] = 'testdatabase'
DATABASES['default']['USER'] = 'testuser'
DATABASES['default']['PASSWORD'] = '1'
DATABASES['default']['HOST'] = '127.0.0.1'
DATABASES['default']['PORT'] = '5432'
```

参考资料：
1. [Writing your first Django app, part 2](https://docs.djangoproject.com/en/3.1/intro/tutorial02/)
2. [How to manage local vs production settings in Django?](https://stackoverflow.com/a/15315143)
