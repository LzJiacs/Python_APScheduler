# Python_APScheduler
APScheduler的全称是Advanced Python Scheduler。它是一个轻量级的 Python 定时任务调度框架。APScheduler 支持三种调度任务：固定时间间隔，固定时间点（日期），Linux 下的 Crontab 命令。同时，它还支持异步执行、后台执行调度任务。

# Install package
```py
In terminal/cmd:
pip install APScheduler
conda install APScheduler
```

# ***三步
1. 新建一个schedulers
    （1）新建调度器schedulers
    
    
    BlockingScheduler : 调度器在当前进程的主线程中运行，也就是会阻塞当前线程
    
    
    BackgroundScheduler : 调度器在后台线程中运行，不会阻塞当前线程
    
    
2. 添加一个调度任务
3. 运行调度任务


```py
# 每两秒报时简单示例代码：（副线程任务，需要有主任务在运行才能调度）part.1
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
        
        
# 每两秒报时简单示例代码：（主线程任务） part.2
import datetime
import time
from apscheduler.schedulers.blocking import BlockingScheduler

def timeTask():
    print(datetime.datetime.utcnow().strftime("%Y-%m-%d %H:%M:%S.%f")[:-3])

if __name__ == '__main__':
    # 创建schedulers
    scheduler = BlockingScheduler()
    
    # 添加调度任务
    # 调度方法：timeTask, 触发器：interval(间隔), 间隔时长 2 秒
    scheduler.add_job(timeTask, 'interval', seconds=2)
    # 启动
    scheduler.start()
```    
# 三类触发器：
1. date触发器

| 参数        | 说明         | 
| -------------- | -------------- |
| run_date (datetime 或 str)      | 作业的运行日期或时间       |
|timezone (datetime.tzinfo 或 str)   | 指定时区        |

```python
from datetime import datetime
from datetime import date
from apscheduler.schedulers.background import BackgroundScheduler
 
def job_func(text):
    print(text)
 
scheduler = BackgroundScheduler()
# 在 2017-12-13 时刻运行一次 job_func 方法
scheduler .add_job(job_func, 'date', run_date=date(2017, 12, 13), args=['text'])
# 在 2017-12-13 14:00:00 时刻运行一次 job_func 方法
scheduler .add_job(job_func, 'date', run_date=datetime(2017, 12, 13, 14, 0, 0), args=['text'])
# 在 2017-12-13 14:00:01 时刻运行一次 job_func 方法
scheduler .add_job(job_func, 'date', run_date='2017-12-13 14:00:01', args=['text'])
 
scheduler.start()
```

2. intervalc 触发器 固定时间间隔触发。 (part.1 part.2)

| 参数        | 说明         | 
| -------------- | -------------- |
| weeks (int)      | 间隔几周   |
|days (int)   | 间隔几天        |
|hours (int)| 间隔几小时|
|minutes (int)| 间隔几分钟|
|seconds (int)| 间隔几秒|
|start_date (datetime/str)|开始时间 |
|end_date (datetime/str)| 结束时间|
|timezone (datetime.tzinfo/str)|时区|

3. cron 触发器 在特定时间周期性地触发，和Linux crontab格式兼容。它是功能最强大的触发器。

| 参数        | 说明         | 
| -------------- | -------------- |
| year (int / str)      | 年 4位数字   |
|month (int / str)| 月(1-12)|
|day (int / str)| 日(1-31)|
|week (int / srt)|周（1-53） |
| days_of_week (int)   | 周内第几天或星期几  (范围0-6 或者 mon,tue,wed,thu,fri,sat,sun)      |
|hour (int)| 时(连续变量：0-23、离散变量用“，”分割：0-10，10-23，1，2，3...)|
|minute (int)| 0-59|
|second (int)| 0-59|
|start_date (datetime / str)|开始时间(包含) |
|end_date (datetime / str)| 结束时间(包含)|
|timezone (datetime.tzinfo / str)|时区|

```python
import time
from apscheduler.schedulers.blocking import BlockingScheduler

scheduler = BlockingScheduler()


@scheduler.scheduled_job('interval', seconds=5)
def job1():
    t = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(time.time()))
    print('job1 --- {}'.format(t))


@scheduler.scheduled_job('cron', second='*/7')
def job2():
    t = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(time.time()))
    print('job2 --- {}'.format(t))


scheduler.add_job(job1, 'interval', seconds=5)
# 等同于 @scheduler.scheduled_job修饰器 修饰器不需要传任务参数，直接把接下来的def作为任务定义

scheduler.start()
```


![cron 算术表达式参数](https://img-blog.csdnimg.cn/20190522111526316.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hjX3pob3U=,size_16,color_FFFFFF,t_70 "cron 算术表达式参数")

