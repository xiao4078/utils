﻿# 1.django-rest-swagger==0.3.10

Django==1.11

djangorestframework==3.7.3

支持YAML docstrings

支持python3


# 2.docker

## dynamic_mount_docker_volume

给正在运行的Docker容器动态绑定卷组（动态添加Volume）

## event_monitor.py

subprocess.Popen 实时获取docker events事件()

# 3. django + celery低耦合使用

django和celery的低耦合使用

基于类的celery任务定义


# 4. 使用Nginx代理s3，动态生成缩略图并缓存

通过 {domain}/{uri}?s={size}实现获取指定大小缩略图

原图：`localhost/u/1523562/avatar`

缩略图：`localhost/u/1523562/avatar?s=200`

缩略图：`localhost/u/1523562/avatar?s=100`


# 5. zerorpc 使用

打印调用日志：打出所有的调用请求、参数和返回值，或写入日志

检测文件改变自动重启： autoreload, 更新代码不用重启服务，使用werkzeug的run_with_reloader函数实现


# 6. logger 日志输出格式配置

```
import logging
import logging.config

import configs

# 应用初始化时加载logger配置
logging.config.dictConfig(configs.LOGGER_CONFIG)

# 各文件使用
logger = logging.getLogger(__name__)

logger.info('teste')
try:
    do_something()
except Exception as e:
    logger.error('error', exc_info=True)
    process_error()


```
output:   ` [2020-10-07 15:47:43 +0000] [118] [INFO] Redis cluster started`


# 7. 全表id倒序分页

根据id倒序（即创建时间倒叙）分页，适用简单场景

使用max获取最大id，从最后一条数据向前分页
通过id倒序向前推进使用 `<=` 和 `limit` 结合的方式优化分页加载
eg: select * from yourtable where id <= seek_id order by id desc limit 20;

### Using

```
from pagination.pagination import WholeTableIdReversePagination

queryset = YourModel.objects.all()
pagination = WholeTableIdReversePagination()
result = list(pagination.paginate_queryset(queryset, request))
```

#### DRF Using

```
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'pagination.pagination.WholeTableIdReversePagination',
}
```

#### Request:
```
GET https://api.example.org/accounts/?page_seek_id=100
or
GET https://api.example.org/accounts/
```

#### Response:
```
HTTP 200 OK
{
    "count": 20
    "next": "https://api.example.org/accounts/?page_seek_id=80",
    "previous": none,
    "results": [
       …
    ]
}
```


## 8. Django原生sql使用Paginator分页
```
from pagination.utils import QueryWrapper


sql = 'select id, username, first_name from auth_user'
queryset = QueryWrapper(sql)

count = queryset.count() 
data = queryset.all()

# 在Django中使用
from django.core.paginator import Paginator
pages = Paginator(queryset, per_page=10)
page = pages.page(page_no) # 获取某页数据


# 在django rest framework中使用
page = self.paginate_queryset(queryset)
results = self.get_paginated_response(page).data
print(results)

>>>
{
	"count": 25,
	"next": "http://127.0.0.1:8888/test/?page=2",
	"previous": null,
	"results": [{
		"id": 11349230,
		"username": "张三",
		"phone": "1440182340944",
	
	}, {
		"id": 11344204,
		"username": "李四",
		"phone": "1440182333431",
	},..
}


```

## 9. elasticsearch中文分词Dockerfile（elasticsearch-ik:6.5.4）

## 10. cassandra(python)的CQL和ORM使用方式

## 11. redis实现锁
