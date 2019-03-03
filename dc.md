## java中的双锁模式

double-checked是为了减少锁的消耗，在请求锁之前先check锁是否

比如下面的单例模式，但是锁的粒度太大。

```java
// Correct but possibly expensive multithreaded version
class Foo {
private Helper helper;
public synchronized Helper getHelper() {
if (helper == null) {
helper = new Helper();
}
return helper;
}
}
```

```java
// Works with acquire/release semantics for volatile in Java 1.5 and later
// Broken under Java 1.4 and earlier semantics for volatile
class Foo {
private volatile Helper helper;
public Helper getHelper() {
Helper localRef = helper;
if (localRef == null) {
synchronized (this) {
localRef = helper;
if (localRef == null) {
helper = localRef = new Helper();
}
}
}
return localRef;
}
}
```