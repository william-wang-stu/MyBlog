---
title: 算法练习（一）
date: 2018-03-15 19:31:48
tags:
- Algorithm
- Binary Search
- Binary Tree
categories:
- Algorithm
---

<blockquote class="blockquote-center">我认为计算机病毒应该当作生命。它道出了人性的某些方面：那就是，迄今为止我们所创造出的生命的唯一的形式纯粹是破坏性的。 我们照自己的形象创造生命。</blockquote>

![](algorithm-one/top.jpg)

### 前言

> 开始回顾算法了，先打算把数据结构相关的一系列操作回顾一下。昨天今天就看了两个算法，结合着最优练的，是二叉树遍历和二分搜索。

### 二叉树的遍历。

在我看来，二叉树的遍历就是递归。可有人说了，`应该有非递归算法啊？`没错，但是。。递归方式是用函数栈递归，而迭代（非递归）是用普通栈递归，只不过是自己手动实现的罢了。

#### 前序遍历

> 前序遍历顺序是`根，左，右`，哈哈哈记得很清楚，毕竟高中就记过，但不会代码实现，只能用大脑的拓扑结构计算出来。哈哈哈

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public List<Integer> preorderTraversal(TreeNode root){
    List<Integer> list = new ArrayList();
    if(root==null)  return list;
    Stack<TreeNode> stack = new Stack();
    stack.push(root);
    while(!stack.empty()){
        TreeNode node = stack.pop();
        list.add(node.val);
        if(node.right!=null) stack.push(node.right);//右子树在栈底
        if(node.left!=null) stack.push(node.left);
    }
    return list;
}
```

#### 后序遍历

> 后序遍历顺序是`左，右，根`，仔细一看，这不就是和`根右左`相反吗。这正好。
```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
 public List<Integer> postorderTraversal(TreeNode root){
     List<Integer> list = new ArrayList();
     if(root==null)  return list;
     Stack<TreeNode> stack = new Stack();
     stack.push(root);
     while(!stack.empty()){
         TreeNode node = stack.pop();
         list.add(node.val);
         if(node.left!=null) stack.push(node.left);
         if(node.right!=null) stack.push(node.right);
     }
     return Collections.reverse(list);
 }
```

#### 中序遍历

> 根本思路还是一样，模拟递归栈。不过和前两者不太一样了。他的顺序是`左根右`，首先伸到深处的左子树。

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
 public List<Integer> inorderTraversal(TreeNode root){
     List<Integer> list = new ArrayList();
     if(root==null)  return list;
     Stack<TreeNode> stack = new Stack();
     TreeNode temp = root;
     while(temp!=null||!stack.empty()){
         while(temp!=null){
             stack.push(temp);
             temp=temp.left;//左子树深处
         }
         TreeNode node = stack.pop();
         list.add(node.val);
         temp = node.right;
     }
     return list;
 }
```


### 二分查找

> 二分查找时间复杂度为** O(log<sub>2</sub>n) **。前提是查找已经排过序的序列。算法思想就是给定序列开始和结束的下标，找到中间值与查找值比较，然后确定下一次寻找范围是在左边部分还是右边部分，然后重复此步骤，直到找到。`没找到返回下标-1`。

```Java
public int binarySearch(int[] nums,int target){
    int left = 0;
    int right = nums.length-1;
    while(left<=right){
        int middle = left + (right-left)/2;//避免使用加法溢出
        if(nums[middle]>target){
            right = middle - 1;
        }else if(nums[middle]<target){
            left = middle + 1;
        }else{
            return middle;
        }
    }
    return -1;
}
```

### 嗯呢，Over。。。

未完，To Be Continued……