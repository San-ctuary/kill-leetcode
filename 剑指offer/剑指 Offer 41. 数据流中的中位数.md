#   [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

通过两个优先队列存储数据，small队列为大根堆，big为小根堆

- 如果small和big中元素个数相等则先将元素插入small中然后去除small中最大的元素放到big中。
- 如果small和big中元素不相等（big中元素个数大于small中元素个数）则先将元素插入big中然后去除big中最大的元素放到small中。

以上操作保证big中元素始终大于small中元素

```java
class MedianFinder {

    public PriorityQueue<Integer> small;
    public PriorityQueue<Integer> big;
    public int size;

    public MedianFinder() {
        this.small = new PriorityQueue<>((a,b) -> b - a);
        this.big = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        if(small.size() == big.size()) {
            small.offer(num);
            big.offer(small.poll());
        } else {
            big.offer(num);
            small.offer(big.poll());
        }
        size++;
    }
    
    public double findMedian() {
        if(size % 2 == 0)
            return (small.peek() + big.peek()) / 2.0;
        else    
            return big.peek();
    }
}
```

python中通过heapq实现小根堆，实现大根堆则把输入的元素取反，取出的时候再取反即可

```python
class MedianFinder:


    def __init__(self):
        """
        initialize your data structure here.
        """
        self.small = []
        self.big = []
        self.size = 0


    def addNum(self, num: int) -> None:
        if(len(self.small) == len(self.big)):
            heapq.heappush(self.small,-num)
            heapq.heappush(self.big,-heapq.heappop(self.small))
        else:
            heapq.heappush(self.big,num)
            heapq.heappush(self.small,-heapq.heappop(self.big))
        self.size += 1


    def findMedian(self) -> float:
        if self.size % 2 == 0:
            return (-self.small[0] + self.big[0]) / 2.0
        else:
            return self.big[0]

```

