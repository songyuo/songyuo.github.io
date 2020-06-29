---
title: 刷leetcode之三天打鱼两天晒网之一直晒网
date: 2019-11-18 20:28:27
tags:
- leetcode
categories:
- 理论基础

---

# 写在前面

之前一直在美国站刷题（其实也就刷了个位数道），后来又换到中文站上去了。

自己是真的懒，明明什么都不会，还不去做。

等到要找工作了，才发现自己要GG了。

每天咸鱼的生活其实也不一定很有乐趣。

忙里偷闲才有幸福的感觉。

虽然一直这么想，但我还是那条咸鱼。

*曾经有一段美好的青春摆在我的面前，我没有珍惜，等我失去了才后悔莫及，尘世间最痛苦的事莫过于此。如果上天能够给我一个再来一次的机会，我会每天都去刷一道leetcode。如果非要给刷题加上一个期限，我希望是-----------一万年。*

<hr/>

# 随时添补的想法

1. 链表由于其结构，很适合使用递归，遇到链表问题时，不妨使用递归试一试；
2. 删除链表结点有两种方法：第一种是找到要删除的链表的前一个结点，然后修改其指针p.next=p.next.next；第二种方法是找到被删除的结点，复制其后一个节点的值到被删除的结点，然后令p.next=p.next.next，**不过需要牢记此法的弊端，当被删除的结点是最后一个结点时，此法失效**。
3. 新建一个结点指向链表的头部，是个很好用的手段，可以解决针对头部结点的操作所带来的麻烦。
4. 有数字相加，考虑大数问题；
5. 牵扯到倒序问题的记得用栈，例如链表倒序问题；
6. 别忘了return，你咋回事，经常忘记，这是个问题，最好剖析一下深层原因
7. 1不是质数，最小的质数是2！！！！
8. s1循环移位的结果是s1s1的子字符串
9. 空串和null都要考虑
10. 旋转字符串，穷举法，循环标记的使用
11. ascil码 字母先大写后小写
12. Java的lambda表达式中外围变量不可改变，见书或者笔记
13. [字符串同构](https://leetcode-cn.com/problems/isomorphic-strings/)下标要加1
14. [回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)，从中心向两边扩展，很棒；索引检查要排在索引引用前面
15. [字母移位](https://leetcode-cn.com/problems/shifting-letters/) 大数？取余？哪里取？
16. BFS 图的单源最短路径问题
17. 骚操作or (char c = 'a'; c <= 'z'; c++)  

<hr/>

#  具体问题

## Robot Bounded In Circle :

美国站第1041题

等效转化：

- 把曲曲折折的路径合成为一个向量
- 左转一次等于右转三次

<hr/>

## Heaters

美国站第475题

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

## 合并两个有序链表

[力扣21题](https://leetcode-cn.com/problems/merge-two-sorted-lists/ )

```java
//我的递归代码
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null)
            return l2;
        if(l2==null)
            return l1;
        ListNode node = null;
        if(l1.val<l2.val){
            node = l1;
            node.next = mergeTwoLists(l1.next, l2);
        }
        else{
            node = l2;
            node.next = mergeTwoLists(l1, l2.next);
        }
        return node;
    }
}
//别人的递归代码
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) {
            return l2;
        }
        if(l2 == null) {
            return l1;
        }

        if(l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;  //可以起到剪枝的作用
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
//感受到什么没有
```

##  链表求和

[力扣445](https://leetcode-cn.com/problems/add-two-numbers-ii/description/)

- 倒序问题想一想栈，链表倒序要想一想头插法
- 当心进位，最后虽然加完了但是可能有进位，不要忘记
- 几个两点，见代码注释

```java
//别人的代码
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) { 
        Stack<Integer> stack1 = new Stack<>();
        Stack<Integer> stack2 = new Stack<>();
        while (l1 != null) {
            stack1.push(l1.val);
            l1 = l1.next;  //亮点，后面用不到了l1了，直接移动就可以了
        }
        while (l2 != null) {
            stack2.push(l2.val);
            l2 = l2.next;
        }
        
        int carry = 0;
        ListNode head = null;
        while (!stack1.isEmpty() || !stack2.isEmpty() || carry > 0) { //条件亮点
            int sum = carry;   //亮点，省略了一步
            sum += stack1.isEmpty()? 0: stack1.pop(); //亮点
            sum += stack2.isEmpty()? 0: stack2.pop();
            ListNode node = new ListNode(sum % 10);
            node.next = head;
            head = node;
            carry = sum / 10; //亮点，比我的选择表达式要好
        }
        return head;
    }
}
//我的代码
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode p = l1;
        boolean carry = false;
        Stack<Integer> stack1 = new Stack<>();
        Stack<Integer> stack2 = new Stack<>();
        while(p!=null){
            stack1.push(p.val);
            p = p.next;
        }
        p = l2;
        while(p!=null){
            stack2.push(p.val);
            p = p.next;
        }
        p = null;
        while(!stack1.isEmpty()&&!stack2.isEmpty()){
            int num1 = stack1.pop();
            int num2 = stack2.pop();
            int sum = carry?(num1+num2+1):(num1+num2);
            ListNode node = new ListNode(sum%10);
            node.next = p;
            p = node;
            carry = sum>9?true:false;
        }
        while(!stack1.isEmpty()){
            int num = stack1.pop();
            int sum = carry?(num+1):(num);
            ListNode node = new ListNode(sum%10);
            node.next = p;
            p = node;
            carry = sum>9?true:false;
        }
        while(!stack2.isEmpty()){
            int num = stack2.pop();
            int sum = carry?(num+1):(num);
            ListNode node = new ListNode(sum%10);
            node.next = p;
            p = node;
            carry = sum>9?true:false;
        }
        if(carry){
            ListNode node = new ListNode(1);
            node.next = p;
            p = node;
        }
        return p;
    }
}
//逆序用栈；小心大数；倒序用头插。
```

## 字符串轮转

[力扣](https://leetcode-cn.com/problems/string-rotation-lcci/)

[讲解KMP的文章](https://www.zhihu.com/question/21923021)

```java
//字符串朴素匹配， 在主串中逐个匹配，不越字符
public boolean isFlipedString(String s1, String s2) {
    if(s1==null||s2==null||s1.length()!=s2.length())
        return false;
    s1 = s1 + s1;
    int len1 = s1.length(), len2 = s2.length(), i = 0, j = 0;
    while(i<len1&&j<len2){
        if(s1.charAt(i)==s2.charAt(j)){
            i++;
            j++;
        }
        else{
            j = 0;
            i = i - j + 1;
        }
    }
    return j==len2;
}
//KMP办法
public boolean isFlipedString(String s1, String s2) {
    if(s1==null||s2==null||s1.length()!=s2.length())
        return false;
    int[] next = getNext(s2);
    s1 = s1 + s1;
    int i = 0, j = 0;
    while(i<s1.length()&&j<s2.length()){
        if(s1.charAt(i)==s2.charAt(j)){
            i++;
            j++;
        }
        else{
            i = i + j + 1;
            j = next[j];
        }
    }
    return j == s2.length();
}

public int[] getNext(String s){
    int maxLength = 0;  //以当前字符做结尾的子串的真前缀和真后缀相匹配的最大长度
    int[] next = new int[s.length()];
    //第一个字符不存在真前缀和真后缀，自动初始化为0不用管它
    for(int i=1;i<s.length();i++){
        while(maxLength>0&&s.charAt(maxLength)!=s.charAt(i))
            maxLength = next[maxLength-1];
        if(s.charAt(maxLength)==s.charAt(i))
            maxLength++;
        next[i] = maxLength;
    }
    return next;
}
//API
public boolean isFlipedString(String s1, String s2) {
    return s1.length()==s2.length()&&(s1+s1).contains(s2);
}
```

## 最长回文串

[力扣409题](https://leetcode-cn.com/problems/longest-palindrome/)

```java
//我的代码
public int longestPalindrome(String s) {
    boolean hasSingle = false;
    HashMap<Character, Integer> map = new HashMap<>();
    for(int i=0;i<s.length();i++)
        map.merge(s.charAt(i), 1, Integer::sum);
    int count = 0;
    for(Map.Entry<Character, Integer> entry:map.entrySet()){
        count += entry.getValue()/2;
        if(!hasSingle) hasSingle = entry.getValue()%2 != 0;
    }
    return count*2 + (hasSingle?1:0);
}
//cyc代码，亮点在判断是否含有单个字符，而且用数组做似乎会快一些
public int longestPalindrome(String s) {
    int[] cnts = new int[256];
    for (char c : s.toCharArray()) {
        cnts[c]++;
    }
    int palindrome = 0;
    for (int cnt : cnts) {
        palindrome += (cnt / 2) * 2;
    }
    if (palindrome < s.length()) {
        palindrome++;   // 这个条件下 s 中一定有单个未使用的字符存在，可以把这个字符放到回文的最中间
    }
    return palindrome;
}
```

## 单词接龙Ⅱ

[力扣](https://leetcode-cn.com/problems/word-ladder-ii/)

[很棒的题解](https://leetcode-cn.com/problems/word-ladder-ii/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-3-3/)

- 如果单纯的用dfs的话，需要设置最小搜索深度，并在搜索中不断更新，如果不设置搜索深度，遇到有连通子图的情况，就会沿着环不断地搜索下去；
- 为了加快效率，可以借助bfs；由于bfs的特性，它可以在第一次找到目的结点的时候就找到最小搜索深度（最短路径），而dfs则不一定；有了最小深度后，用dfs在这个深度内去搜索即可；但是，仍然会造成许多无意义的搜索;
- 之所以会造成这么多无意义的搜索，是因为有的结点会与之前或者同层的结点相连，相当于又倒回去重新搜索。这时候，我们可以利用bfs确定每一层的结点，得到一层结点后就把这层结点删除，这就可以使结点只与下一层的结点相连，然后用dfs在确定深度内搜索即可，只会进不会退，可以保证高效率
- 能否只用bfs解决问题呢？dfs由于可以使用栈，它可以在不同递归函数中保存不同状态的路径，回溯之后就可以找到上一层路径；而bfs在搜索中不用递归，导致结点在队列删除之后路径丢失；换一种思路就可以解决问题，我们可以在bfs的队列中保存路径而不是保存结点，在扩充队列中用路径中最后一个结点进行搜索，在得到一层的结点后删除之，同样也可以解决问题；
- 双向bfs+dfs，有点难搞，建议看题解，忘了的话debug一下。

<hr/>

# 随时间增多的牢骚

1. 2020.6.19.  81道，我为什么还是只刷了这么点题；
2. 2020.6.29，又开始咸鱼了，要找不到工作了呀，但是我还是死猪不怕开水烫的样子，有个小伙伴会好一点吧，孤军奋战不太好；

<hr/>



