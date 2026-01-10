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



