[代码随想录](https://www.bilibili.com/video/BV1QB4y1D7ep?vd_source=82d188e70a66018d5a366d01b4858dc1&spm_id_from=333.788.videopod.sections)

# 常用技巧

`join` 是胶水，不是容器。

join:

```python
新字符串 = "分隔符".join(要被连接的列表)
```

```python
a = ['1', '2', '3', '4']
print('.'.join(a))
#输出
#1.2.3.4
```



find:

```python
list.index(x[, start[, end]])
```

元素第一次出现的位置

deque:双向列表

```
q=deque()
q.popleft()
```



# 数组

## 704:二分法

通常使用左闭右开区间 [left,right)

```python
left=0
right=n
while left<right:
    middle=(left+right)//2
    if nums[middle]<target:
        right=middle
    if nums[middle]>target:
        left=middle-1
    
```

区间是左闭右开,因此仅`left<right`是非法的,使用这个作为循环条件

## 27:移除元素

快慢双指针法

## 977有序数组平方

中间开始,左右双指针

##  209 长度最小的子数组

快慢双指针

## 59.螺旋矩阵II   TODO

```python
class Solution:
    def generateMatrix(self, n: int,left=0) -> List[List[int]]:
        res=[[0] * n for _ in range(n)]
        startx, starty = 0, 0               # 起始点
        loop, mid = n // 2, n // 2          # 迭代次数、n为奇数时，矩阵的中心点
        count = 1                           # 计数

        for offset in range(1,loop+1):
            for i in range(startx,n-offset):
                res[starty][i]=count
                count+=1
            for i in range(starty,n-offset):
                res[i][n-offset]=count
                count+=1
            for i in range(n-offset,startx,-1):
                res[n-offset][i]=count
                count+=1
            for i in range(n-offset,starty,-1):
                res[i][starty]=count
                count+=1
            startx+=1
            starty+=1
    
        if n%2==1:
            res[n//2][n//2]=n**2
        return res
```

通过循环嵌套模拟,建议多看看类似的题目

# 链表



## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(next=head)  
        slow = dummy                 
        right = head
        
        # 快指针先走n步
        for i in range(n):
            right = right.next
        
        while right:
            right = right.next
            slow = slow.next  # 移动慢指针
        
        # 删除目标节点：慢指针的下一个节点就是要删的倒数第n个节点
        slow.next = slow.next.next
        
        return dummy.next     # 修正3：返回虚拟头节点的next，永远不要return head！！！
```

原链表的 `head` 节点**有可能就是被删除的那个节点**,通过dummy节点避免这个问题

## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head
        
        while True:
            if not fast or not fast.next:
                return None
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                break
        
        slow = head
        
        while fast != slow:
            slow = slow.next
            fast = fast.next
        
        return fast
```

公式可以去题目里面看一下,有点抽象



# 哈希表

查询元素是否出现过

## [242.有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

`Counter(s)`用于生成一个字典,key:value 为key对应的出现次数

标准做法是下面这样使用哈希表,但是`Counter(s)`是封装好的高级函数,有空看一下

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        n=len(s)
        dis=defaultdict(int)
        dit=defaultdict(int)
        if len(t)!=n:
            return False
        for i in range(n):
            dis[s[i]]+=1
            dit[t[i]]+=1
        if dis==dit:
            return True
        return False
```



## [349. 两个数组的交集](https://www.bilibili.com/video/BV1ba411S7wu/?share_source=copy_web&vd_source=9e952e3695aa7bfc9ff110afee9f3d34)

1.用数组充当hash table

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        arr=[0 for _ in range(1001)]
        res=[]
        for i in nums1:
            arr[i]+=1
        for j in nums2:
            if arr[j]:
                res.append(j)
                arr[j]=0
        return res
```

## 2.通过字典映射

```python
class Solution:
    def intersection(self, nums1,nums2):
        hash_dict = {}
        for i in nums1:
            if i in hash_dict:
                hash_dict[i] += 1
            else:
                hash_dict[i] = 1
        result_set = set()
        for j in nums2:
            if j in hash_dict.keys(): #hash_dict.keys():key变成list
                result_set.add(j)
                del hash_dict[j]
        return list(result_set)
```

## 3.set哈希表

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        hashtable=set()
        res=set()
        for i in nums1:
            hashtable.add(i)
        
        for j in nums2:
            if j in hashtable:
                res.add(j)
                
        return list(res)
```

## [1. 两数之和](https://leetcode.cn/problems/two-sum/)

用字典作为哈希表,用数组同理

```python
class Solution:
    def twoSum(self, nums, target):
        hash_map = {}
        for i, num in enumerate(nums):
            temp=target-num
            if temp in hash_map:
                return [hash_map[temp],i]
            else:
                hash_map[num]=i
```

## [454. 四数相加 II ](https://leetcode.cn/problems/4sum-ii/)   TODO

1.暴力:超时

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        count=0
        for i in nums1:
            for j in nums2:
                for k in nums3:
                    for l in nums4:
                        if i+j+k+l==0:
                            count+=1
        return count
```

**2.分治**

n1和n2组成一个hash table,n3和n4组成一个,然后再求组合为0 , 复杂度O$(n^2)$ (四数相加变为两数之和)

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        res=0
        count1=defaultdict(int)
        count2=defaultdict(int)
        for i in nums1:
            for j in nums2:
                count1[i+j]+=1
        for k in nums3:
            for l in nums4:
                    count2[k+l]+=1

        for i in count1.keys():
            for j in count2.keys():
                if i+j==0:
                    res+=count1[i]*count2[j]
        return res
```

3. 优化:使用python提供的更优方法

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        d1=defaultdict(int)
        count=0
        for i in nums1:
            for j in nums2:
                d1[i+j]+=1
        for i in nums3:
            for j in nums4:
                	
                    count+=d1[-(i+j)]
        return count
```

## [15. 三数之和 ](https://leetcode.cn/problems/3sum/)  TODO

固定一个值,然后移动另外两个作为双指针(我固定中间值这个做法有点蠢),排序O(nlogn) ,固定双指针$O(n^2)$

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        ans = []
        n = len(nums)
        for i in range(n-2):
            #跳过重复的i，避免重复解
            if i > 0 and nums[i] == nums[i-1]:
                continue
            j,k=i+1,n-1
            while j<k:
                s = nums[i] + nums[j] + nums[k]
                if s == 0:
                    ans.append([nums[i], nums[j], nums[k]])
                    # 【修正3】找到解后再去重 ✅ 正确时机，先去重再移指针，避免重复解
                    while j < k and nums[j] == nums[j+1]:
                        j += 1
                    while j < k and nums[k] == nums[k-1]:
                        k -= 1
                    # 【修正1】核心！找到解后必须移动指针，否则死循环
                    j += 1
                    k -= 1
                elif s > 0:
                    k -= 1
                elif s < 0:
                    j += 1
            i+=1
        return ans
```



```python
 使用字典

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        # 找出a + b + c = 0
        # a = nums[i], b = nums[j], c = -(a + b)
        for i in range(len(nums)):
            # 排序之后如果第一个元素已经大于零，那么不可能凑成三元组
            if nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i - 1]: #三元组元素a去重
                continue
            d = {}
            for j in range(i + 1, len(nums)):
                if j > i + 2 and nums[j] == nums[j-1] == nums[j-2]: # 三元组元素b去重
                    continue
                c = 0 - (nums[i] + nums[j])
                if c in d:
                    result.append([nums[i], nums[j], c])
                    d.pop(c) # 三元组元素c去重
                else:
                    d[nums[j]] = j
        return result
```

## [18. 四数之和](https://leetcode.cn/problems/4sum/) TODO

**n数之和就是在2数之和基础上套上循环**,降低一个n复杂度

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        n=len(nums)
        nums.sort()
        if n<4:
            return[]
        res=[]
        
        for i in range(n-3):
            #防止重复
            if i > 0 and nums[i] == nums[i-1]:
                continue

            for j in range(i+1,n-2):
                #防止重复
                if j > i+1 and nums[j] == nums[j-1]:
                    continue
                    
                left1,right1=j+1,n-1
                while left1<right1:
                    s=nums[i]+nums[j]+nums[left1]+nums[right1]
                    if s==target:
                        res.append([nums[i],nums[j],nums[left1],nums[right1]])
                        ##防止重复
                        while left1<right1 and nums[left1]==nums[left1+1]:
                            left1+=1
                        left1+=1
                        ##防止重复
                        while  right1>left1 and  nums[right1]==nums[right1-1]:
                            right1-=1
                        right1-=1
                    elif s<target:
                        while  left1<right1 and nums[left1]==nums[left1+1]:
                            left1+=1
                        left1+=1
                    elif s>target:
                        while right1>left1 and nums[right1]==nums[right1-1]:
                            right1-=1
                        right1-=1
        return res
                
```



2.set做法,性能比上面略差,空间O(n)

```python
class Solution(object):
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        # 创建一个字典来存储输入列表中每个数字的频率
        freq = {}
        for num in nums:
            freq[num] = freq.get(num, 0) + 1
        
        # 创建一个集合来存储最终答案，并遍历4个数字的所有唯一组合
        ans = set()
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                for k in range(j + 1, len(nums)):
                    val = target - (nums[i] + nums[j] + nums[k])
                    if val in freq:
                        # 确保没有重复
                        count = (nums[i] == val) + (nums[j] == val) + (nums[k] == val)
                        if freq[val] > count:
                            ans.add(tuple(sorted([nums[i], nums[j], nums[k], val])))
        
        return [list(x) for x in ans]
```

# 字符串

## [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        n=len(s)
        for i in range(n//2):
            s[i],s[n-i-1]=s[n-i-1],s[i]
        
```

## [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/) TODO

### 自己的代码:

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        n=len(s)
        times=n//(2*k)
        #2k部分
        for i in range(times):
            for j in range(k):
                # 你的代码里所有这种交换写法都会触发 TypeError
                #Python 中 str 类型的字符串是不可修改的，不能通过 s[index] = 新值 的方式修改某个位置的字符，也不能直接交换两个位置的字符。
                #应该先变成list再吃力
                s[i*2*k+j],s[i*2*k+k-j-1]=s[i*2*k+k-j-1],s[i*2*k+j]
        #剩余部分x
        i=times
        x=n%(2*k)
        if x >k:
            for j in range(k):
                s[i*2*k+j],s[i*2*k+k-j-1]=s[i*2*k+k-j-1],s[i*2*k+j]
        else :
            for j in range(x):
                
                s[i*2*k+j],s[n-j-1]=s[n-i-1],s[i*2*k+j]
                
                #正确的应该是
                s[i*2*k+j],s[n-j-1]=s[n-j-1],s[i*2*k+j]
```

修改后

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        n = len(s)
        # 修正1：字符串转列表，解决不可变问题
        s_list = list(s)
        times = n // (2 * k)
        # 处理完整的2k段
        for i in range(times):
            start = i * 2 * k
            # 反转[start, start+k)区间，用切片替代手动交换，简洁高效
            s_list[start:start+k] = s_list[start:start+k][::-1]
        # 处理剩余部分
        x = n % (2 * k)
        start = times * 2 * k
        if x > k:
            # 剩余>=k个，反转前k个
            s_list[start:start+k] = s_list[start:start+k][::-1]
        else:
            # 剩余<k个，反转全部剩余，修正2：修正你的下标笔误问题
            s_list[start:n] = s_list[start:n][::-1]
        # 列表转回字符串
        return ''.join(s_list)
```





Python 的**切片语法**可以一行实现任意区间的反转，简洁 + 高效，比如 `arr[a:b] = arr[a:b][::-1]` 就能反转列表中 `[a,b)` 区间的元素。



## [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/) TODO

本题思路总结：1.先去除字符串多余的空格 2.再将去除空格后的字符串整个反转 3。最后在反转后的字符串中反转单词

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # s.split()用于将字符串按照指定的分隔符切割，返回一个字符串列表
        return " ".join(reversed(s.split()))
```



## [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

```
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        slist=list(s)
        n=len(s)
        if n<1:
            return True
        for i in range(1,n//2+1):
            if n%i==0:
                if s==s[:i]*(n//i):
                    return True
        return False
```

## [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

kmp算法实现,用内置函数套路一下

1.直接暴力匹配 O(m*n)

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n = len(haystack)
        m=len(needle)
        for i in range(n):
            if needle==haystack[i:i+m]:
                return i
        return -1
```

2.找第一个子串:

✅ 1. `str.find(sub)` → 推荐（本题最优解）

- 找不到子串 → 返回 `-1`
- 无报错风险，**完全匹配本题需求**，是最常用的查找方法

✅ 2. `str.index(sub)` → 慎用（本题不能用）

- 找到子串 → 返回第一次出现的下标（和 find 一致）
- **找不到子串 → 直接抛出 ValueError 异常** ❌
- 本题如果用 `return haystack.index(needle)`，会在匹配失败时报错，无法通过测试用例！

✅ 3. `str.rfind(sub)` → 反向查找

- 功能：**从右往左**查找子串第一次出现的下标
- 例：`"ababa".rfind("aba")` → 返回 2 （最右侧的 aba 起始下标）

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return  haystack.find(needle)
```

# 栈+队列

## [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

难度不大

```python
class Solution:
    def isValid(self, s: str) -> bool:
        arr=[]
        dic={'(':')','{':'}','[':']'}
        for i in s:

            if i in [')','}',']']:
                if not arr:
                    return False
                else:
                    j=arr.pop()
                    if dic[j]!=i:
                        return False
                    else:
                        continue
            else:
                    arr.append(i)
        if not arr:
            return True
        return False
```

优化一下写法

```python
class Solution:
    def isValid(self, s: str) -> bool:
        dic = {'(' : ')', '[' : ']', '{' : '}', '?' : '?'}
        stack = ['?']
        for c in s:
            if c in dic: stack.append(c)
            elif dic[stack.pop()] != c: return False
        return len(stack) == 1 
```



## [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/) TODO

使用栈和队列处理更轻松

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        arr=[]
        sList=list(s)
        for i in s:
            if arr:
                if arr[-1]==i:
                    arr.pop()
                else:
                    arr.append(i)
            else:
                arr.append(i)
        return ''.join(arr)
```





## [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        arr=[]
        caculat=[ '+','-','*' , '/']
        for i in tokens:
            if i not in caculat:
                arr.append(int(i))
            else:
                c1=arr.pop()
                c2=arr.pop()
                if i=='+':
                    arr.append(c1+c2)
                elif i=='-':
                    arr.append(c2-c1)
                elif i=='*':
                    arr.append(c1*c2)
                elif i=='/':
                    arr.append(int(c2/c1))
        return arr[0]
```





## [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/) TODO

暴力,会超时(O(n*k)

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        arr=[]
        n=len(nums)
        #可能有重复最大值,删去时要注意
        for i in range(n-k+1):
            arr.append(max(nums[i:i+k]))
        return arr
```

参考答案

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        # 
        q=deque()
        ans=[]
        for i ,x in enumerate(nums):
            #淘汰过期元素
            while q and q[0]<=i-k:
                q.popleft()
            #淘汰无用元素
            # 保证队列里的下标对应的数值严格单调递减
            while q and nums[q[-1]]<x:
                q.pop()
            q.append(i)
            #当 i 增长到 k-1 时，
            # 说明第一个完整的窗口（长度为 k）已经形成了
            if i>=k-1:
                ans.append(nums[q[0]])
        return ans
```





#### 双端队列操作

```python
from collections import deque

# 创建一个空的deque
d = deque()

# 添加元素
d.append('a')
d.appendleft('b')
print(d) # 输出: deque(['b', 'a'])

# 移除元素
d.pop()
d.popleft()
print(d) # 输出: deque([])
```



## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/) TODO

o(NlogN) 整个排序

```python
import collections

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        c = collections.Counter(nums)
        # 按照频率 (c[x]) 降序排序，取前 k 个
        sorted_keys = sorted(c.keys(), key=lambda x: c[x], reverse=True)
        return sorted_keys[:k]
```

O(NlogK) 堆排序

```python
import collections
import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        c = collections.Counter(nums)
        # heapq.nlargest 自动维护堆，根据 c.get (即频率) 来排序
        # 通过 key 参数来指定一个函数，这个函数会在每个元素上被调用，
        # 其返回值将作为排序的依据
        return heapq.nlargest(k, c.keys(), key=c.get)
```

使用collections的内置函数

```python
import collections

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        c = collections.Counter(nums)
        # most_common(k) 返回如 [(1, 3), (2, 2)] 的形式
        # 我们只需要取元组的第一个元素（即数字本身）
        return [item[0] for item in c.most_common(k)]
```

# 二叉树

## 递归

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        
        def dfs(node):
            if node is None:
                return
            res.append(node.val)
            
            dfs(node.left)
            dfs(node.right)
        dfs(root)
        return res
```

前中后序接近,不赘述

## 迭代

借助**栈**实现



### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

```python
from collections import deque

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        t=[]
        if not root:
            return []
        t.append(root)

        while t:
            temp=t.pop()
            
            if temp.right:
                t.append(temp.right)
            if temp.left:
                t.append(temp.left)
            res.append(temp.val)
        
        return res
```



### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

```python

class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        t=[]
        if not root:
            return []
        t.append(root)

        while t:
            temp=t.pop()
            if temp.left:
                t.append(temp.left)
            if temp.right:
                t.append(temp.right)
            res.append(temp.val)
        res.reverse()
        return res
```

先序后序比较简单,中序会复杂一点

[94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res =[]
        stack=[]
        if not root:
            return []
        #中节点,用于避免在左之前弹出
        cur=root
        while cur or stack:
            if cur:
                stack.append(cur)
                cur=cur.left
            else:
                # 左边处理完,剩下中和右 , 中加入result
                cur=stack.pop()
                res.append(cur.val)
                cur=cur.right
        return res

```



## [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/) TODO

层序遍历应使用队列

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        res = []
        # 语义化命名：queue=当前层队列，next_level=下一层节点，cur_level=当前层数值
        queue = [root]
        next_level = []
        cur_level = []

        while queue or next_level:
            cur_level = []
            next_level = []
            while queue:
                node = queue.pop(0)
                cur_level.append(node.val)
                if node.left:
                    next_level.append(node.left)
                if node.right:
                    next_level.append(node.right)
            res.append(cur_level)
            queue = next_level.copy()
        return res
```

官方解法,省去复制的时间和保存下一行的空间,更优秀

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        res = []
        if not root: return res
        queue = [root]
        while queue:
            level_size = len(queue) # 获取当前层节点数
            cur_level = []
            for _ in range(level_size): # 只遍历当前层的节点
                node = queue.pop(0)
                cur_level.append(node.val)
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
            res.append(cur_level)
        return res
```



## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def inv(node):
            #交换左右节点
            if not node:
                return
            temp=node.left
            node.left=node.right
            node.right=temp
        if not root:
            return
        inv(root)
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```





## [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/) TODO

后序遍历: 下层的信息向上层返回,比较内侧和外侧
` return compare(l.left,r.right) and compare(l.right,r.left)`

```python
class Solution:
    
            
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        def compare(l,r):
            if l and (not r): return False
            elif (not l) and  r: return False
            elif (not l) and (not r): return True
            elif l.val != r.val : return False
            #把下面的答案递归上来
            return compare(l.left,r.right) and compare(l.right,r.left)
        if not root:
            return True
        return compare(root.left,root.right)
```



## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        def re_dep(l,r):
            if not l and not r :return 0
            if l and not r: return re_dep(l.left,l.right)+1
            if not l and  r: return re_dep(r.left,r.right)+1
            if l and r: return max( re_dep(l.left,l.right),re_dep(r.left,r.right),)+1
        if not root:
            return 0
        return re_dep(root.left,root.right)+1
```



## [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

跟上面差不多,从下往上传递信息

```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        def func(l,r):
            if not l and not r : return 0
            elif r and not l :return 0
            elif l and not r: return 1
            return func(l.left,l.right)+func(r.left,r.right)+2
        return func(root.left,root.right)+1
```

### [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        res=[]
        def xiaqian(pre,node):
            now=pre+"->"+str(node.val)
            if node.left: xiaqian(now,node.left)
            if node.right: xiaqian(now,node.right)
            if not node.left and not node.right: res.append(now)
        if not root:
            return []
        start=str(root.val)
        if  root.left:xiaqian(start,root.left)
        if  root.right:xiaqian(start,root.right)
        if not root.left and not root.right: res.append(start)
        return res
```





## [404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)

```python
class Solution:
    res=0
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        self.res=0
        #flag=1为左,flag=0为右
        def sum_left_leaf(node,flag):
            if not node : return
            if node.left:sum_left_leaf(node.left,1)
            if node.right:sum_left_leaf(node.right,0)
            #左叶子
            if flag and ( not node.left) and (not node.right):
                self.res+=node.val
            
        if not root: return 0
        sum_left_leaf(root.left,1)
        sum_left_leaf(root.right,0)
        return self.res
```



## [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

递归法

```python
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        def func(node,level):
            #叶子
            if not node.left and not node.right:
                return node.val,level

            #只有单边
            if node.left and not node.right:
                return func(node.left,level+1)
            if node.right and not node.left:
                return func(node.right,level+1)
            #都有
            lv,llevel=func(node.left,level+1)
            rv,rlevel=func(node.right,level+1)
            if llevel<rlevel:
                return rv,rlevel
            return lv,llevel
        if not root: return 0
        v,l=func(root,1)
        return v
```



## [112. 路径总和](https://leetcode.cn/problems/path-sum/)

递归+全局变量

```python
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        flag=False
        def dfs(node,target):
            nonlocal flag
            if flag or not node: return 
            if node.left: 
                dfs(node.left,target-node.val)
            if node.right: 
                dfs(node.right,target-node.val)
            #满足条件的叶子节点
            if not node.left and not  node.right and target==node.val:
                flag=True
        
        dfs(root,targetSum)
        return flag
```

优化一下

```python
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root: return False
        if not root.left and not root.right:
            return  targetSum==root.val
        return self.hasPathSum(root.left,targetSum-root.val) or self.hasPathSum(root.right,targetSum-root.val)
```



## [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

自底向上递归建树

```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if not inorder : return None
        mid=postorder.pop()
        idx=inorder.index(mid)
        l=self.buildTree(inorder[:idx],postorder[:idx])
        r=self.buildTree(inorder[idx+1:],postorder[idx:])
        root=TreeNode(mid,l,r)
        return root
```





## [654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)

和上一题一样,这里复杂度为O(N^2)

```python
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums: return
        idx=nums.index(max(nums))
        l=self.constructMaximumBinaryTree(nums[:idx])
        r=self.constructMaximumBinaryTree(nums[idx+1:])
        root=TreeNode(nums[idx],l,r)
        return root
```

优化:**单调栈**:单调递减

```python
# 654. 最大二叉树 - 单调栈解法
# 时间复杂度: O(n)，每个元素最多入栈出栈一次
# 空间复杂度: O(n)，单调栈最多存储 n 个节点

class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        """
        核心思想：维护一个【单调递减栈】
        
        栈中存储的是 TreeNode 节点，从栈底到栈顶，节点值严格递减。
        
        对于每个新元素 num：
        1. 如果 num < 栈顶元素：num 应该成为栈顶元素的【右子节点】
           (因为 num 在栈顶元素的右边，且比它小)
        2. 如果 num > 栈顶元素：不断弹出栈顶，最后弹出的元素成为 num 的【左子节点】
           (被弹出的元素在 num 的左边，且比 num 小，所以是 num 的左子树)
        """
        stack = []  # 单调递减栈，存储 TreeNode 节点
        
        for num in nums:
            # 为当前数字创建新节点
            node = TreeNode(num)
            
            # 关键步骤1：处理所有比当前值小的栈顶元素
            # 它们都应该成为当前节点的左子树的一部分
            while stack and stack[-1].val < num:
                # 弹出栈顶节点，它将成为当前节点的左子节点
                # (最后一个被弹出的节点才是直接左子节点)
                node.left = stack.pop()
            
            # 关键步骤2：如果栈不为空，说明栈顶元素比当前值大
            # 当前节点应该成为栈顶元素的右子节点
            if stack:
                stack[-1].right = node
            
            # 将当前节点入栈
            stack.append(node)
        
        # 栈底元素就是整棵树的根节点 (最大值对应的节点)
        return stack[0] if stack else None
```

与递归解法对比：递归 O(n²) 最坏，单调栈 O(n)



## [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)

```python
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        root=TreeNode()

        if not root1:
            root= root2
        elif not root2:
            root= root1
        elif root1 and root2:
            root.val= root1.val+root2.val
            root.left= self.mergeTrees(root1.left,root2.left)
            root.right= self.mergeTrees(root1.right,root2.right)
        return root
```



## [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

```python
class Solution:
    temp=float('-inf')
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True

        l=self.isValidBST(root.left)
        if  self.temp>=root.val:
            return False
        else : 
            self.temp=root.val
        if l:
            r=self.isValidBST(root.right)
        return l and r
```

优化一下,避免多余计算

```python
class Solution:
    temp=float('-inf')
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True

        l=self.isValidBST(root.left)
        if l:
            if  self.temp>=root.val:
                return False
            else : 
                self.temp=root.val
                r=self.isValidBST(root.right)
        return l and r
```





## [530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

中序遍历,和上面差不多

```python
class Solution:
    cha=1e5
    last=-1e5
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        
        def mid(node):
            if not node:
                return
            mid(node.left)
            self.cha=min(self.cha,node.val-self.last)
            self.last=node.val
            mid(node.right)
        mid(root)
        return int(self.cha) 
```



## [501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

```python
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        self.count=0
        self.max_count=0
        self.curr=int(-1e5-1)
        self.res=[]
        def mid(node):
            if not node:
                return
            mid(node.left)
            if node.val!=self.curr:
                self.curr = node.val 
                self.count=1
            else:
                self.count+=1
            if self.count>self.max_count:
                self.max_count=self.count
                self.res=[node.val]
            elif self.count==self.max_count:
                self.res.append(node.val)
            mid(node.right)
        mid(root)
        return self.res
```



## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        self.res=None
        def func(node,p,q):
            flag=0
            if self.res or not node:
                return False
            if node.val==p.val or node.val==q.val:
                flag+=1
            if func(node.left,p,q): flag+=1
            if func(node.right,p,q): flag+=1
            if flag==1: return True
            elif flag==2: 
                self.res=node
                return False
            return False
        func(root,p,q)
        return self.res
```

优化,不借助外部变量

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 1. 终止条件 / Base Case
        # 如果越过叶子节点，或者找到了 p 或 q 其中一个，直接返回当前节点
        if not root or root == p or root == q:
            return root
        
        # 2. 递归 / Recursion
        # 去左边找
        left = self.lowestCommonAncestor(root.left, p, q)
        # 去右边找
        right = self.lowestCommonAncestor(root.right, p, q)
        
        # 3. 处理 / Conquest
        # 情况A: 左右两边都有返回值，说明 p 和 q 分别在两侧
        # 此时 root 就是最近公共祖先
        if left and right:
            return root
        
        # 情况B & C: 只有一边有返回值（或者两边都空）
        # 说明 p 和 q 都在这一边（或者都没找到）
        # 直接把找到的结果向上传递
        if left:
            return left
        return right
```



## [701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

```python
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        temp=TreeNode(val)
        if not root:
            return temp
        node=root
        while node:
            if node.val>val :
                if not node.left:
                    node.left=temp
                    return root
                else:
                    node=node.left
            if node.val<val :
                if not node.right:
                    node.right=temp
                    return root
                else:
                    node=node.right
```

## [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)  TODO

```python
class Solution:
    #利用递归返回下一层的值,从而避免寻找父节点
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None
        # 辅助函数：寻找以 node 为根的树中的最小值（一直往左走）
        def get_min( node):
            while node and node.left:
                node = node.left
            return node
        #递归找到key
        if root.val>key:
            root.left= self.deleteNode(root.left,key)
        elif root.val<key:
            root.right=self.deleteNode(root.right,key)
        else:
            #叶子
            if not root.left and not root.right:
                return None

            #只有其中一个节点
            elif not root.left:
                return root.right
            elif  not root.right:
                return root.left

            #左右节点都有
            else:
                #左下节点替代当前节点,继续递归删除最后的节点
                node0=get_min(root.right)
                root.val=node0.val
                root.right=self.deleteNode(root.right,node0.val)
                return root
```



## [669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/) TODO

根据当前节点的值，果断**放弃掉不需要的那一半子树**

```python
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return
        #舍弃半边子树和自己
        if root.val<low:
            root.left=None
            root=self.trimBST(root.right,low,high)
        elif root.val>high:
            root.right=None
            root=self.trimBST(root.left,low,high)
        else:
            root.right=self.trimBST(root.right,low,high)
            root.left=self.trimBST(root.left,low,high)
        return root
```



## [108. 将有序数组转换为平衡二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/) todo

以中间作为分界,不断递归中序分治

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        mid = len(nums)//2
        root = TreeNode(nums[mid])

        root.left=self.sortedArrayToBST(nums[:mid])
        root.right=self.sortedArrayToBST(nums[mid+1:])

        return root
```





## [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

```python
class Solution:
    a=0
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: return
        self.convertBST(root.right)
        root.val+=self.a
        self.a=root.val
        self.convertBST(root.left)

        return root
```

优化一下,确保调用时会清空self.pre

```python
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        self.pre = 0
        def dfs(node):

            if not node:
                return 
            dfs(node.right)
            node.val = self.pre + node.val
            self.pre = node.val
            dfs(node.left)
        
        dfs(root)
        return root
```

# 回溯算法

组合 : [1,2,3,4,]有多少种两两组合方式
切割 : 切割后的子串都是回文子串
子集 : [1,2,3,4,]有多少种组合方式
组合 : 无顺序的排列
棋盘 : n皇后,解数独 

一般会带着多个参数进行回溯,在构建过程中不断添加

收集结果 : **叶子节点** or **所有节点**

##### 三部曲 : 处理 , 递归 , 回溯

**三部曲 : 处理(本层处理),递归(深层),回溯(从递归返回)**

## [77. 组合](https://leetcode.cn/problems/combinations/)  TODO

利用回溯,达到类似嵌套for循环的做法

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res=[]
        path=[]
        def fun(s,e,k):
            #start,end,k 递归深度
            if len(path)==k:
                #path[:] path进行深拷贝,避免后续修改
                res.append(path[:]) 
                return
            else:
                for i in range(s,e+1):
                    path.append(i)
                    fun(i+1,e,k)
                    path.pop()
        fun(1,n,k)
        return res
```

## 上述代码可以优化,剪枝一下有

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res=[]
        path=[]
        def fun(s,e,k):
            #start,end,k 递归深度
            n=len(path)
            #剪枝
            if k-n>e-s+1:
                return
            if n==k:
                res.append(path[:]) 
                return
            else:
                for i in range(s,e+1):
                    path.append(i)
                    fun(i+1,e,k)
                    path.pop()
        fun(1,n,k)
        return res
```



```python
return list(combinations(range(1,n+1),k))
```





## [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res=[]
        path=[]
        def func(start):
            if len(path)==k and sum(path)==n:
                res.append(path[:])
            else:
                for i in range(start,10):
                    path.append(i)
                    func(i+1)
                    path.pop()
        func(1)
        return res
```

剪枝优化(自己写的)

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res=[]
        path=[]
        def func(start):
            s,lenth=sum(path),len(path)
            if s>n or 10-start<k-lenth :
                return
            
            if len(path)==k and sum(path)==n:
                res.append(path[:])
            else:
                for i in range(start,10):
                    path.append(i)
                    func(i+1)
                    path.pop()
        func(1)
        return res
```





## [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

这里用递归会产生内存问题,比较麻烦

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        KEY = {'2': ['a', 'b', 'c'],
               '3': ['d', 'e', 'f'],
               '4': ['g', 'h', 'i'],
               '5': ['j', 'k', 'l'],
               '6': ['m', 'n', 'o'],
               '7': ['p', 'q', 'r', 's'],
               '8': ['t', 'u', 'v'],
               '9': ['w', 'x', 'y', 'z']}
        path=[]
        res=[]
        n=len(digits)
        def func(start):
            if start==n:
                res.append(''.join(path[:]))
                return
            for i in KEY[digits[start]]:
                path.append(i)
                func(start+1)
                path.pop()
        func(0)
        return res
```

也就打表恶心了点

## [39. 组合总和](https://leetcode.cn/problems/combination-sum/) TODO



```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res=[]
        cur=[]
        candidates.sort()
        def func(start,target0):
            if target0==0:
                res.append(cur[:])
                return
            elif target0<2:
                return
            else:
                #通过start,避免重复组合
                for idx,i in enumerate(candidates[start:]):
                    if target<i : break #剪枝
                    cur.append(i)
                    func(idx+start,target0-i)
                    cur.pop()
        func(0,target)
        return res
```



## [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/)

**注意：**解集不能包含重复的组合。 

自己写的:通过count迫使连续部分全取,从而避免了重复计算

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res=[]
        path=[]
        candidates.sort()
        n=len(candidates)
        def func(start,target):
            if target==0:
                res.append(path[:])
            else:
                for i,num in enumerate(candidates[start:]):
                    if target<num:
                        break
                    count=1
                    while i+start+count<n and candidates[i+start]==candidates[i+start+count]:
                        path.append(num)
                        count+=1

                    path.append(num)
                    func(start+i+count,target-num*count)
                    
                    for i in range(count):
                        path.pop()

        func(0,target)
        return res
```

法2:暴力法去重:超时(用tuple和set进行去重)

法3:剪枝仅保留一个元素,删去其他重复元素,效率更高(递归变少 , 因为第一个肯定包含所有的
而且直接使用索引,更省内存

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res=[]
        path=[]
        candidates.sort()
        n=len(candidates)
        def func(start,target):
            if target==0:
                res.append(tuple(path[:]))
            for i in range(start, len(candidates)):
                if candidates[i] > target: break
                #减去重复的部分
                if i > start and candidates[i] == candidates[i - 1]: continue
                path.append(candidates[i])
                func(i+1,target-candidates[i])
                path.pop()

        func(0,target)


        return  res
```



# 回溯切割

## [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)  TODO

使**每个子串都是 回文串** 。返回 `s` 所有可能的分割方案 

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res=[]
        n=len(s)
        path=[]
        def func(start_idx):
            if start_idx==n: 
                res.append(path.copy())
                return
            #切割后续子串
            for i in range(start_idx,n):
                #切割下来的子串,
                sub_string=s[start_idx:i+1]
                #数组操作,判断是否为回文子串
                if sub_string==sub_string[::-1]:
                    path.append(sub_string)
                    func(i+1)
                    path.pop()
        func(0)
        return res

```



错误答案:只考虑了奇数部分,没考虑偶数,而且**题目理解有问题**

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res=[]
        n=len(s)
        def func(mid):
            path=s[mid]
            res.append(path)
            count=min(mid,n-1-mid)
            for i in range(1,count+1):
                c=s[mid+i]
                if s[mid-i]==c:
                    path=c.join(path).join(c)
                    res.append(path)
                else:
                    return
        for i in range(n):
            func(i)
        return res
```



## [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)  TODO



#### 自己写的错误答案

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        res=[]
        n=len(s)
        path=[]
        def func(start_idx):
            #
            if len(path)>4 or (len(path)==4 and start_idx!=n) or start_idx>n-1:
                return
            if start_idx==n and len(path)==4: 
                res.append(path.copy())
                return
            

            #切割子串
                #前导0
            if s[start_idx]=='0':
                path.append(s[start_idx])
                func(start_idx+1)
                path.pop()
                return

            #无前导0
            for i in range(start_idx,start_idx+3):
                #判断是否小于255(前导0已取出)
                

                #切割下来的子串,
                sub_string=s[start_idx:i+1]
                num=int(sub_string)

                if num>255:
                    return
                else:
                    path.append(sub_string)
                    func(i+1)
                    path.pop()
                #没有前导0
                
        func(0)
        res1=[]
        result_strings = ['.'.join(p) for p in res]
        return result_strings

```

#### 修改以后



```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        res=[]
        n=len(s)
        path=[]
        def func(start_idx):
            if len(path) > 4:
                return
            
            # 如果遍历到了字符串末尾
            if start_idx == n:
                # 只有刚好切成4段才算成功
                if len(path) == 4:
                    res.append(path.copy())
                return

            #无前导0,min(start_idx + 3, n)用于避免越界
            for i in range(start_idx,min(start_idx + 3, n)):
                # 1. 检查前导 0：如果长度 > 1 且以 '0' 开头，非法

                sub_string=s[start_idx:i+1]
                num=int(sub_string)
                if len(sub_string) > 1 and sub_string.startswith('0'):
                    return
                if num>255:
                    return
                else:
                    path.append(sub_string)
                    func(i+1)
                    path.pop()
                #没有前导0
                
        func(0)
        res1=[]
        #.join(p)有返回值,上面的做法不正确(不修改原来的p)
        result_strings = ['.'.join(p) for p in res]
        return result_strings

```

优雅写法

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        # 预先判断：如果字符串长度不在合法范围内 [4, 12]，直接返回空
        if len(s) < 4 or len(s) > 12:
            return []
            
        res = []
        n = len(s)
        
        # path: 当前已经收集到的段列表
        # start: 当前处理到的字符串索引
        def backtrack(start, path):
            # --- 核心优化：强力剪枝 ---
            # 计算还需要凑几个段
            needs = 4 - len(path)
            # 计算还剩多少个字符
            remain = n - start
            
            # 如果剩余字符 不够分 (remain < needs) 
            # 或者 剩余字符 太多了 (remain > 3 * needs)，直接剪枝
            if remain < needs or remain > 3 * needs:
                return
            
            # 终止条件：如果不满足上面的剪枝，且 path 满了 4 个，
            # 说明肯定正好分完（由剪枝逻辑保证），直接加入结果
            if len(path) == 4:
                res.append(".".join(path))
                return

            # --- 循环逻辑 ---
            # 尝试截取长度 1, 2, 3
            for length in range(1, 4):
                # 越界检查
                if start + length > n:
                    break
                
                sub = s[start : start + length]
                
                # 1. 前导 0 检查：长度 > 1 且首位是 '0'，直接结束循环
                # (因为再往后切肯定也是 '0' 开头，都没戏)
                if length > 1 and sub[0] == '0':
                    break
                    
                # 2. 数值检查：大于 255，直接结束循环
                # (因为再往后切数值更大)
                if int(sub) > 255:
                    break
                
                # 递归
                # 技巧：直接传 path + [sub]，创建新列表传参
                # 这样利用了函数调用栈的特性，省去了 path.pop() 的显式回溯步骤
                backtrack(start + length, path + [sub])

        backtrack(0, [])
        return res
```



