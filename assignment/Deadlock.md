#lab4:死锁


###产生死锁的四个必要条件：
1. 互斥条件：一个资源每次只能被一个进程使用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3. 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
4. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。

这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。

###实验代码
```
class A{
     synchronized void methodA(B b){
     b.last();
       }

synchronized void last(){
    System.out.println("Inside A.last()");
    }

}

class B{
    synchronized void methodB(A a){
    a.last();
    }

synchronized void last(){
    System.out.println("Inside B.last()");
    }
}
//这里是主函数的时间轴，当t.start(),之后，线程t就被插入到调度队列里，当调度到他的时候，就跑run()里面的代码
class Deadlock implements Runnable{
    A a=new A();

    B b=new B();
   
   Deadlock(){
       Thread t = new Thread(this);
       int count = 10000;

       t.start();//线程t开始
       while(count-->0);//等待count值的时间，
       a.methodA(b);
       }
   //runable运行时调用的方法
   public void run(){
       b.methodB(a);
       }

   public static void main(String args[]){
      new Deadlock();
       }
  }
```
####实验流程：
1. 把上面的代码抄到 Deadlock.java里面,保存
2.  javac Deadlock.java	
3.  linux 系统下书写 Deadlock.sh文件
```
#!/bin/bash

for(( c=1;c<=1000;c++))
do
	echo "$c times"
	java Deadlock
done
```
4. 使用  ```.\\Deadlock.sh```命令运行。
###运行结果：
![](http://ww4.sinaimg.cn/mw690/a44300b5gw1f9mzor3cg5j20jo0cutaq.jpg)
在第67次结束。



###实验代码产生死锁的原因
理解产生死锁的原因首先要了解关键字synchronized的概念：

> * 关键字 synchronized
当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。
当一个线程访问object的一个synchronized同步代码块或同步方法时，其他线程对object中所有其它synchronized同步代码块或同步方法的访问将被阻塞。

结合代码我们可以知道methodA，methodB, A.last 和 B.last 都拥有关键字 synchronized，所以在同时间段内，methodA和A.last，methodB和B.last 分别只能被一个线程访问。

由实验代码可以看出，在程序中共涉及到两个线程：主线程Deadlock和子线程t。
首先主线程Deadlock运行。
Deadlock会创建子线程t,然后等待count的while循环结束，之后执行a.methodA(b)语句,调用A类的methodA函数。

如果在准备调用但还没调用B类的last()函数时由于时间片结束，主线程会退出CPU。

此时子线程占用CPU执行b.methodB(a)函数，执行B类的methodB()函数，若此时时间片结束，程序会回到主线程Deadlock。Deadlock想要继续调用调用b.last()却发现B类的被声明为synchronized的methodB()函数正在被调用着，因此Deadlock被阻塞。

等时间片用完切换到子线程t,此时要调用a.last()，由于methodA()函数正在被Deadlock线程调用着，t线程阻塞。

等时间片结束回到Deadlock线程。Deadlock 想要继续调度 b.last()，但是methodB被占用，Deaklock被阻塞，周而复始，产生死锁。
 
流程示意图:
![](http://ww2.sinaimg.cn/mw690/a44300b5gw1f9myl9z15ij209o0a6dgb.jpg)

 
