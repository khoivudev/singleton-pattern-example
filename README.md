## SINGLETON-PATTERN
The Singleton Pattern ensures a class has only one instance, and provides a global point of access to it.
### Class diagram
![Alt Text](https://github.com/khoivudev/singleton-pattern-example/blob/master/design/class-diagram.png)
### DEAL WITH MULTITHREADING
#### Problem:
![Alt Text](https://github.com/khoivudev/singleton-pattern-example/blob/master/multithreading-problem.png)
#### Solutions
##### Solution 1: Synchronize the getInstance() method
```java
public static synchronized Singleton getInstance(){}
```
* Problem: 
    * once we’ve set the uniqueInstance variable to an instance of Singleton, we have no further need to synchronize this method. 
    * After the first time through, synchronization is totally unneeded overhead!
    * Just keep in mind that synchronizing a method can decrease performance by a factor of 100, so if a high-traffic part of your code begins using getInstance(), you may have to reconsider.

##### Solution 2: Use eager instantiation
* If application always creates and uses an instance of the Singleton or theoverhead of creation and runtime aspects of the Singleton are not onerous, you may want to create your Singleton eagerly.
* The JVM guarantees that the instance will be created before any thread accesses the static uniqueInstance variable.
```java
private static Singleton uniqueInstance = new Singleton();
public static Singleton getInstance() {
    return uniqueInstance;
}
```

##### Solution 3: Double-checked locking
* With double-checked locking, we first check to see if an instance is created, and if not, THEN we synchronize. 
* This way, we only synchronize the first time through, just what we want.
```java
private volatile static Singleton uniqueInstance;
public static Singleton getInstance() {
    if(uniqueInstance == null) {
        synchronized (Singleton.class) {
            if (uniqueInstance == null) {
                uniqueInstance = new Singleton();
            }
        }
    }
    return uniqueInstance;
}
```
* Problem:
    * Double-checked locking doesn’t work in Java 1.4 or earlier!