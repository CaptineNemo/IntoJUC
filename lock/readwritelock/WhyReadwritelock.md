# Why we need readwritelock
Assume that there is some data, with read-operations and write-operations on it.Now we want to make sure read-operations and write-operations are thread-safe.

## Version 1.0 read and write without lock
```java
public class Cache {
    // the data that we want to make sure thread-safe
    private Map<String, Object> map = new HashMap<String, Object>();

    public Object get(String key) {
        return map.get(key);
    }

    public Object put(String key, Object value) {
        return map.put(key, value);
    }
}
```
There will be several problems:
* there might be more than one PUT operations at the time;
* return value of GET operations might be modifying at that time;

## Version 2.0 read and write with one exclusive lock
```java
public class Cache {
    // the data that we want make to sure thread-safe
    private Map<String, Object> map = new HashMap<String, Object>();
    // exclusive lock
    ReentrantLock lock = new ReentrantLock();

    public Object get(String key) {
        try{
            lock.lock();
            return map.get(key);
        }finally {
            lock.unlock();
        }

    }

    public Object put(String key, Object value) {
        try{
            lock.lock();
            return map.put(key, value);
        }finally {
            lock.unlock();
        }
    }
}
```
With ReentrantLock, every operation whether read or write, need to get lock first, then has permit to do read or write.
But there is a problem —— putting every operation with one exclusive lock makes them fully serialized, whick make the performance bad.
Is there need to lock within every operation？
No, obviously there is no need to lock within read operations.We can let many threads to read at the same time, that wouldn't cause unconsistency problems.
Of course if a write thread get the lock, all reads should be blocked.So here is the rules:
* read threads can "share the lock" together
* read thread blocks write threads
* write thread block other write threads
* write thread block other read threads

That's exactly what readwritelock does, ReentrantReadWriteLock, is one implementation of ReadWriteLock provided by JUC.
Look at the changes with ReentrantReadWriteLock:
## Version 3.0 read with read lock and write with write lock
```java
public class Cache {
    // the data that we want to make sure thread-safe
    private Map<String, Object> map = new HashMap<String, Object>();
    // readwrite lock
    private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();
    private Lock r = rwl.readLock();
    private Lock w = rwl.writeLock();

    public Object get(String key) {
        try {
            r.lock();
            return map.get(key);
        } finally {
            r.unlock();
        }
    }

    public Object put(String key, Object value) {
        try {
            w.lock();
            return map.put(key, value);
        } finally {
            w.unlock();
        }
    }
}
```
## Conclusion
We want to make read and write operations thread-safe, but avoid unnecessary serialization within read operations.
So we use readwritelock, read operation needs to get read lock, write operation needs to get write lock.
Read lock and write lock must be implemented in one class, within which readLock() allow other readLock() but blocks
writeLock(), writeLock() blocks readLock() and other writeLock().

About real implementation of readwritelock in JUC, see [ReentrantReadWriteLock](ReentrantReadWriteLock.md);