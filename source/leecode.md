---
title: leetcode
date: 2019-11-18 20:28:27
tags:
- leetcode
categories:
- 理论基础
---

1. Robot Bounded In Circle (1041):

   等效转化：

   - 把曲曲折折的路径合成为一个向量
   - 左转一次等于右转三次

2.  Heaters(475):

   -  Java 中Arrays.binarySearch() 函数：采用二分法查找元素在数组中的位置，如果未找到，返回  负的（理应插入的位置的下标 + 1）

   - Java 中的~符号，二进制位全部取反，~a = - (a + 1), 因为反码 + 1 = 补码 = 相反数

   - 规划思想，先取每栋房子与临近加热器的距离的最小值，再在这些最小值中取最大值

   - 考虑周全：考虑到房子处于边界位置，此时只有一个临近的加热器

   - 另附python解法：

     ```python
     def findRadius(self, houses, heaters):
         heaters = sorted(heaters) + [float('inf')]
         i = r = 0
         for x in sorted(houses):
             while x >= sum(heaters[i:i+2]) / 2.:
                 i += 1
             r = max(r, abs(heaters[i] - x))
         return r
     ```

     不断比较两个heater中house更靠近谁一些

3.  Two Sum （1）:

   边加边找

4. 

