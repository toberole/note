synrhronized用于同步

锁还有个名字叫做对象监视器。
当多个线程同时请求某个对象监视器时，对象监视器会设置几种状态用来区分请求的线程：
Contention List：所有请求锁的线程将被首先放置到该竞争队列
Entry List：Contention List中那些有资格成为候选人的线程被移到Entry List
Wait Set：那些调用wait方法被阻塞的线程被放置到Wait Set
OnDeck：任何时刻最多只能有一个线程正在竞争锁，该线程称为OnDeck
Owner：获得锁的线程称为Owner
!Owner：释放锁的线程




















