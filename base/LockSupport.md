# Why we need LockSuport
Think about a question: for some reason, I want to wake up a blocked thread, what should I do ?
Further more, is there a simple way to make a thread to block and wake up?  
Regardless of LockSuport, a natual way we thought about is Thread.suspend and Thread.resume, which are both deprecated.
Another way is to make the thread wait on certain object, and call its notify() to wake up the thread, aboviously, it's
unconvenient and high-risk to let client programmers manage lock/unlock and the lock object itself.

That's why we need LockSuport ———— A tool providing a raliable and simple way to block and unblock threads

# Simple usage



# Pratice in juc

