---
title: 剑指offer
date: 2019-11-18 20:28:27
tags:
- 剑指offer
categories:
- 理论基础:
---

1. 重建二叉树：

   递归写法中，如果左右子树未递归完成，则不能返回结点指针。

2. 用两个栈实现队列：

   <font color=red>错误示范<font>

   ```java
   for(int i=0; i<stack1.length;i++){
       stack2.push(stack1.pop());
   }
   ```

   不要在序列长度改变的循环中用它的长度作循环条件！！！

3. 别的：在复习直接插入排序时，for(;a[j]>temp&&j>=0;j--)会导致越界，应当for(;j>=0&&a[j]>temp;j--)

   折半插入排序中，遇到array[mid]==target，应当绕过去，令start=mid+1

   从0开始，length-1就是倒数第一个，以此类推

