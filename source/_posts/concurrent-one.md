---
title: 多线程并发问题（一）
date: 2018-03-13 13:57:14
tags:
- thread
- concurrent
categories:
- thread
---

<blockquote class="blockquote-center">软件就是煤气；会扩张至充满整个容器。</blockquote>

![](concurrent-one/top.jpg)

### 前言

> emmmm，题目来的有点不知所措，不知所措。。。下面是题目。

![](concurrent-one/question.jpg)

### 个人看法

实话，我确实对多线程操作不怎么了解。突然出现这么一个题目确实让我不知所措。上学期Java课了解过多线程，只不过层次比较浅，了解过生产者消费者问题，在操作系统中也了解过信号量互斥锁。但仅限于了解，不能够解决一些实际的问题。

### 这个题目

恩，提议是这样的，共有三个线程，你需要完成以下功能。三个线程中其中一个打印`A`，一个打印`B`，一个打印`C`，要求打印结果以`ABC`为顺序打印10次。

### 我的思路

对于这个题，我打算用Java来写。一开始想用Object类的`wait`和`notify`来解决问题，但有点问题，死锁和多线程共享变量不可见性。可能在查资料前并没能用这种方式来解决，因为思路不够清晰就放弃了。

我的解决思路是这样的：
首先要求三个线程打印不同的字母但顺序必须是一定的，那就需要一个信号量来控制，`哪个信号对应哪个线程打印`，当然这个信号量是多线程的共享变量。另外，要求打印十次，那就需要一个线程共享变量来控制次数。以上便可以解决。

### 代码

> 这是我的代码，可能会有点粗糙，还有些地方未做优化，但也是硬着头皮写完的。

```Java
package concurrent;

public class IcePhoneThread {
    private int flag = 1;//标志信号量
    private volatile int num = 0; //保证次数能够及时共享

    private synchronized void printA(){
        if (num==10){//若已经打印了10次就不再打印
            return;
        }
        System.out.println(num+1+":\nA");
        flag=2;
    }
    private synchronized void printB(){
        System.out.println("B");
        flag=3;
    }
    private synchronized void printC(){
        System.out.println("C");
        flag=1;
        num=num+1;
    }

    private class IceThread extends Thread{//内部类
        private int thid;
        public IceThread(int id){
            this.thid = id;
        } 

        @Override
        public void run() {
            while (num<10){//可能最后一次循环病不起作用，已经进入循环，所以我在printA方法中判断了，以为此方法是加了synchronized的，保证了原子性。
                if (thid==1&&flag==1){
                    printA();
                }else if (thid==2&&flag==2){
                    printB();
                }else if (thid==3&&flag==3){
                    printC();
                }
            }
        }
    }

    public static void main(String[] args) {
        IcePhoneThread icePhoneThread = new IcePhoneThread();
        icePhoneThread.new IceThread(1).start();
        icePhoneThread.new IceThread(2).start();
        icePhoneThread.new IceThread(3).start();
    }
}

```

### 最后

> GG,over! 嗯呢。