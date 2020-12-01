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



#### Pro with extending Thread

* It's straightforward to exted the Thread super class
  * Just override the run() hook method
* All state and methods are consolidated in one place.
  * enables central allocation and management of the thread
  * The design is useful when the thread must be updated during runtime configuration changes
    * e.g., interrupting/restarting a running thread and readding / writing its state

#### Cons with extending Thread

* A subclass must extend the Thread superclass
  * This is restrictive since java only allows one superclass per subclass



#### Pros of implementing Runnable

