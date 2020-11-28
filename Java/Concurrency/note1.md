```java
public calss GCDRunnable implements Runnable {
  public void run() {
    // code to run goes here
  }
}
Runnable gCDruannable = new GCDRunnable();
new Thread(gCDrunnable).start();

// lambda expr
new Thread(()-> {
  // code to run
}).start();
// OR
Runnable r = () -> {
  // code
};
new Thread(r).start();
```



### Join

THe `join()` method allows one thread to wait for another thread to complete.

A simple form of **barrier synchronization**



`void setDaemon()`: Marks thread as a "daemon"

If there is only a daemon thread, the program will shutdown

* `void start()`
  * Allocates thread resources & initiates thread execution by calling the run() hook method
* `void run()`
  * Hood method where user code is supplied
* 



