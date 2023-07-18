# Example
Before read this, we assume you have known What is readwritelock and [Why we need readwritelock](WhyReadwritelock.md).
Here is an usage example of ReentrantReadWriteLock:
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
