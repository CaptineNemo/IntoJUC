# Foundation of JUC
AQS ——> various locks ——> collections and utils ——> frameworks and components ——> concurrency system.   
There is no need to emphasize AbstractQueuedSynchronizer's importantance, Actually, it's one of decisive reasons I build the project.   
Let's go into it!

I will introduce it in these respects:
* core methods 
* main processes
* questions

# core methods

| methods                                      | extra |
|----------------------------------------------|-------|
| [addWaiter](./methods/addWaiter.md)          |    |
| [acquireQueued](./methods/acquireQueued.md)  |    |
| [shouldParkAfterFailedAcquire](./methods/shouldParkAfterFailedAcquire.md)  |    |
| [cancelAcquire](./methods/cancelAcquire.md)  |    |

# process of main operations

| process | extra |
|---------|-------|
| wait    | wait  |
| wait     | wait   |

# questions

| questions                                                                   | extra |
|-----------------------------------------------------------------------------|-------|
| [How does CANCELLED node come up ?](./questions/cancelledCameup.md)         |    |
| [How does CANCELLED node move out the queue ?](./questions/cancelledOut.md) |    |