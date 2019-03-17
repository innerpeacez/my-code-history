划重点：前面一个操作的结果对后续操作是可见的，Happens-before 规则基本上都关于可见性

#### 1. 程序的顺序性规则

一个线程中，按照代码编写的顺序，前面的操作 Happens-Before 于后面的任意操作

#### 2. volatile 变量规则

一个 volatile 变量的写操作 Happens-Before 于后续对这个 volatile 变量的读操作

#### 3. 传递性

A 操作 Happens-Before 于 B 操作，B 操作 Happens-Before 于 C 操作, 那么 A 操作 Happens-Before 于 C 操作

#### 4. 管程中的锁规则

一个锁的解锁 Happens-Before 与后续对个锁的加锁操作

#### 5. 线程 start() 规则

父线程 A 中启动子线程 B 后，父线程 A 在启动子线程 B 之前的操作对子线程 B 可见

#### 6. 线程 join() 规则

父线程 A 等待子线程 B 完成操作之后，子线程 B 的操作对父线程 A 可见