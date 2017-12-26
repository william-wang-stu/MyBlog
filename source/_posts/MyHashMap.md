---
title: 自己写一个HashMap
date: 2017-12-23 23:24:24
categories: 深入理解Java
tags:
- Java
- HashMap
---



### 前言 （Foreword）
> 最近操作系统实验感觉一直在copy，copy。。。嗯，不是感觉。连个HashMap自己都懒得写。许久之前看过【码农翻身】的[《什么是HashMap》](http://mp.weixin.qq.com/s?__biz=MzAxOTc0NzExNg==&mid=2665514069&idx=1&sn=2996d864bbe596d0af763fba3d244fa7&chksm=80d67c16b7a1f500ec6a191eb4a0beac0e95dbd5a7bf8ee01f5ed2cb17960b9ab32c0b965949&mpshare=1&scene=23&srcid=1223e7BQGb04LAH3YZ1zDXAQ#rd
)，推荐一下这个公众号（码农翻身），炒鸡棒！看过这篇推送后一直没有实现。今天终于可以拿起笔来写一写。写的有什么不对的请指出，勿喷。

### 原理 （Theory）
> HashMap是一种以键值对存储数据的数据结构，简单的来说是这样。内部怎么实现的呢？实际上使用一个数组（entries），然后数组中的每一个元素可以看成是一个链表（entry）。当存储一个键值对时，你拿着一个Key和Value，哈希算法会根据你的Key来计算出一个值，我们把这个值当作内部数组（entries）的索引值（index），然后找这个索引值下的元素来存储value值，如果此索引下已经存在元素，但因为每个数组元素（entry）可以看作是链表可以连着存储，故可以插入该索引。只不过这里是头插法，因为某种不可抗力会更有可能查询刚插入的元素。先看一下代码,虽然写的很糟，如果实在不能理解，请看一下【码农翻身】上面给了链接。

### 代码（Code）

#### 这是源代码

```Java
public class MyHashMap<K,V> {
	
	private static int default_length=16;
	private MyEntry<K, V>[] entries;
	
	
	public MyHashMap() {
		super();
		entries = new MyEntry[default_length];
	}
	
	public V put(K key,V value) {
		int index = key.hashCode()%default_length;
		MyEntry<K, V> previous = entries[index];
		for(MyEntry entry=entries[index];entry!=null;entry = entry.next) {
			if(entry.getKey().equals(key)) {
				V oldValue = (V) entry.getValue();
				entry.setValue(value);
				return oldValue;
			}
		}
		MyEntry<K, V> entry = new MyEntry<>(key,value);
		entry.next=previous;
		entries[index] = entry;
		return null;
	}
	
	public V get(K key) {
		int index = key.hashCode()%default_length;
		for(MyEntry<K, V> entry=entries[index];entry!=null;entry=entry.next) {
			if(entry.getKey().equals(key)) {
				return entry.getValue();
			}
		}
		return null;
	}


	private final class MyEntry<K,V> {
		private K key;
		private V value;
		private MyEntry next;
		public MyEntry(K key, V value) {
			super();
			this.key = key;
			this.value = value;
		}
		public MyEntry() {
			super();
		}
		public MyEntry(K key, V value, MyEntry next) {
			super();
			this.key = key;
			this.value = value;
			this.next = next;
		}
		public K getKey() {
			return key;
		}
		public void setKey(K key) {
			this.key = key;
		}
		public V getValue() {
			return value;
		}
		public void setValue(V value) {
			this.value = value;
		}
		public MyEntry getNext() {
			return next;
		}
		public void setNext(MyEntry next) {
			this.next = next;
		}
	}
}
```
#### 这是测试代码
```Java
public class HashTest {

	public static void main(String[] args) {
		MyHashMap<String, Integer> mhm = new MyHashMap<>();
		mhm.put("lmy", 20);
		System.out.println(mhm.get("lmy"));
	}
}
```

#### 这是结果

![](MyHashMap/hashtest.png)
