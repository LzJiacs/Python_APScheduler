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
from apscheduler.schedulers.background import BackgroundScheduler

def timeTask():
    print(datetime.datetime.utcnow().strftime("%Y-%m-%d %H:%M:%S.%f")[:-3])

if __name__ == '__main__':
    # 创建schedulers
    scheduler = BackgroundScheduler()
    
    # 添加调度任务
    # 调度方法：timeTask, 触发器：interval(间隔), 间隔时长 2 秒
    scheduler.add_job(timeTask, 'interval', seconds=2)
    # 启动
    scheduler.start()
    
    while True:
        print(time.time())
        time.sleep(5)
```    

