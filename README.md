# Python_APScheduler
APScheduler的全称是Advanced Python Scheduler。它是一个轻量级的 Python 定时任务调度框架。APScheduler 支持三种调度任务：固定时间间隔，固定时间点（日期），Linux 下的 Crontab 命令。同时，它还支持异步执行、后台执行调度任务。

# Install package
```python
pip install APScheduler
```

# 三步
1. 新建一个schedulers
2. 添加一个调度任务
3. 运行调度任务

# 每两秒报时简单示例代码：
```python
import datetime
import time
from apscheduler.schedulers.backgroud import BackgroudScheduler

def timeTask():
    

