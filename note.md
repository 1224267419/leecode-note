[代码随想录](https://www.bilibili.com/video/BV1QB4y1D7ep?vd_source=82d188e70a66018d5a366d01b4858dc1&spm_id_from=333.788.videopod.sections)

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

