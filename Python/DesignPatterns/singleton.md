```python
class Singleton(object):
  ans = None
  
  @staticmethod
  def get_instance():
    if '_instance' not in Singleton.__dict__:
      Singleton._instance = Singleton()
    return Singleton._instance
  
s1 = Singleton.instance()
s2 = Singleton.instance()

```



threading version

```python
import threading
class Singleton(object):
    _instance_lock = threading.Lock()

    def __new__(cls, *args, **kwargs):
        if not hasattr(Singleton, "_instance"):
            with Singleton._instance_lock:
                if not hasattr(Singleton, "_instance"):
                    Singleton._instance = object.__new__(cls)
        return Singleton._instance
```

