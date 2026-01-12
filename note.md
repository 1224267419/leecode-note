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

