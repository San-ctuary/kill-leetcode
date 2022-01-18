# [剑指 Offer II 057. 值和下标之差都在给定的范围内](https://leetcode-cn.com/problems/7WqeDu/)

## 排序的set

通过滑动窗口加排序后的set来完成对于当前nums[i]  - k元素的查找,二叉平衡树查找和删除都是nlogn

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        TreeSet<Long> tree = new TreeSet<>();
        for(int i = 0;i < nums.length;i++) {
            Long ceiling = tree.ceiling((long)nums[i] - (long)t);
            if(ceiling != null && ceiling <= (long)nums[i] + (long)t)
                return true;
            tree.add((long)nums[i]);
            if(tree.size() > k)
               tree.remove((long)nums[i - k]);
        }
        return false;
    }
}
```

```python
class Solution:
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        from sortedcontainers import SortedList
        sl = SortedList()
        for i in range(len(nums)):
            index = bisect.bisect_left(sl,nums[i] - t)
            if index != len(sl) and sl[index] <= nums[i] + t:
                return True
            sl.add(nums[i])
            if len(sl) > k:
                sl.remove(nums[i - k])
        return False
```

