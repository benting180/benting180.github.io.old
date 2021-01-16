```bash
from threading import Thread
import os
import math
 
 
t1 = time.time()
def calc():
  for i in range(0, 4000000):
    math.sqrt(i)
 
threads = []
 
for i in range(os.cpu_count()):
  print('registering thread %d' % i)
  threads.append(Thread(target=calc))
 
for thread in threads:
    thread.setDaemon(True)
for thread in threads:
  thread.start()
```

```bash
for thread in threads:
    thread.setDaemon(True)
for thread in threads:
    thread.start()
for thread in threads:
    thread.join()
```

with just start(), then join():
the main thread would wait until all thread are finish, then exit()

with just start():
the main thread should just exit once all threads are started. but in practice it does not happen. because there are some non-daemon thread running?

with setDaemon=True, start():
1. main python program can exit when there are no non-daemon thread exist.
… another way of saying:
when threads are daemon, the program can just exit. without caring there are still some daemon thread.

example use case:
some task need to be running at background while main programming is running.
more specifically, say a multiplayer shooter game program, the networking thread would be a daemon thread. sending player’s movement, actions.

# reference:
1. https://laike9m.com/blog/daemon-is-not-daemon-but-what-is-it,97/
2. Threading vs Multiprocessing