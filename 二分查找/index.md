# 数据结构与算法 - 二分查找


### 模板 1 - 二分查找
```
def binary_search(nums, target):
    if len(nums) <= 0:
        return -1
    
    left, right = 0, len(nums) - 1 # 查找区间[left, right]
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

### 模板 2 - 查找左边界
```
def binary_search_left_bound(nums, target):
    if len(nums) <= 0:
        return -1
    
    left, right = 0, len(nums) # 查找空间[left, right)
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] >= target:
            right = mid
        else:
            left = mid + 1

    if left < len(nums) and nums[left] = target:
        return left
    return -1
```

### 模板 3 - 查找右边界
```
def binary_search_right_bound(nums, target):
    if len(nums) <= 0:
        return -1

    
```

### 例题 1 - 寻找两个正序数组的中位数
<https://leetcode-cn.com/problems/median-of-two-sorted-arrays/>
![寻找两个正序数组的中位数](寻找两个正序数组的中位数.png "寻找两个正序数组的中位数")
```
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        k1 = ( len(nums1) + len(nums2) + 1 ) // 2
        k2 = ( len(nums1) + len(nums2) + 2 ) // 2

        # 二分法找第k小数，每次舍去某个数组的一半，因此可用递归实现
        def helper(nums1,nums2,k): # 本质上是找第k小的数
            if(len(nums1) <len(nums2) ):
                nums1, nums2 = nums2 , nums1 #保持nums1比较长
            if(len(nums2)==0):
                return nums1[k-1] # 短数组空，直接返回
            if(k==1):
                return min(nums1[0],nums2[0])  # 找最小数，比较数组首位
            t = min(k//2,len(nums2)) # 保证不上溢
            if( nums1[t-1]>=nums2[t-1] ):
                return helper(nums1 , nums2[t:],k-t)
            else:
                return helper(nums1[t:],nums2,k-t)
        return ( helper(nums1,nums2,k1) + helper(nums1,nums2,k2) ) /2
```

