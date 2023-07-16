# Why we need Unsafe
Assume that you have known some basics about lock and CAS, we know that CAS in code level is based on CAS mechanism
provided by compiler, of course, which provided by OS, then provided by CPU, i.e.
code CAS <—— compiler CAS <—— OS CAS <—— CPU CAS
Back to java, how to use CAS service provided by CPU?
There is the answer —— Unsafe

# Simple usage



# Pratice in juc