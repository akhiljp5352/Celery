# Celery

### 1. [Yigit Guler - Understanding Celery & CeleryBeat](https://www.youtube.com/watch?v=kDoHrFLkahA&t=249s)
- Application, Message queue, workers
- `celery -A tasks worker --loglevel=info`
- Celery beat for periodic monitoring

### 2. [Task Queues: A Celery Story](https://www.youtube.com/watch?v=ceJ-vy7fvus)
- Task queues are queues of tasks (some unit of work), which are distributed by a broker to different workers, who process them asynchronously.
-  Task queue workers run in seperate processes with seperate GILs.
-  Configs that can be handy:
    - `app.task_acks_late=True` Tasks aren't reserved but not acknowledged wuntil task execution finishes
    -  `app.worker_prefetch_multiplier=1` Each worker can only hold one unacknowedged task at a time
- Other task queue libraries than celery: RQ-Redis Queue, Huey, Dramatiq, TaskTiger, Dask
  
`RQ`

  ```
    from redis import Redis
    from rq import Queue
    
    q = Queue(connection=Redis())
    
    def add(x,y)
    return x+y
    
    def client():
    res = add.delay(1,2)
    while not res.is_finished:
      time.sleep(1)
    return res.return_value
    
    def worker():
    os.system("rq worker")
```

`Dramatiq`

```
    import dramatiq
    
    @dramatiq.actor()
    def add(a,b):
      return a+b
    
    def client():
        return add.send(1,2)
    
    def worker():
      os.system("dramatiq module.path")
```

`Huey`

```
    from huey import RedisHuey
    
    huey = RedisHuey()
    
    @huey.task()
    def add(a,b):
      return a+b
    
    def client():
      res = add(1,2)
        return res.get()
    
    def worker():
      os.system("huey_consumer.py module.path")
```

`TaskTiger`

```
    import tasktiger
    
    tiger = tasktiger.TaskTiger()
    
    @tiger.task()
    def add(a,b):
      return a+b
    
    def client():
        return add.delay(1,2)
    
    def worker():
      os.system("tasktiger")
```

`Dask is not a task queue, but provides distributed parallelism in python. Allows you to distribute data operations over a cluster. Provides distributed pandas DataFrames and numpy arrays.`

```
    from dask.distributed import Client
    
    client = Client()
    
    def add(a,b):
      return a+b
    
    def client_fn():
        return client.submit(add, 1,2).result()
    
    def scheduler():
      os.system("dask-scheduler")
    
    def worker():
      os.system("dask-worker")
```

  
  
