# 数据结构与算法 - LRU & LFU



### LRU
```
class LRUCache:

    def __init__(self, capacity):
       self.capacity = capacity
       self.queue = collections.OrderedDict()

    def get(self, key):
        if key not in self.queue:
            return -1
        value = self.queue.pop(key)
        self.queue[key] = value # 将命中的数据重新添加至头部
        return self.queue[key]

    def put(self, key):
        if key in self.queue: # 若命中，则删除老数据
            self.queue.pop(key)
        elif len(self.queue.items()) == self.capacity: # 未命中，且队列满，则删除最后一个数据
            # popitem(last=True), 有序字典的 popitem() 方法移除并返回一个 (key, value) 键值对。
            # 如果 last 值为真，则按 LIFO 后进先出的顺序返回键值对，否则就按 FIFO 先进先出的顺序返回键值对。
            self.queue.popitem(last=False) # 删除最后添加的键值，网上有说随机删除一对并不准确
        
        self.queue[key] = value
```

### LFU
```
class LFUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.key2val = {}
        self.key2freq = {}
        self.freq2keys = collections.defaultdict(collections.deque)
        self.min_freq = 1

    def increase_freq(self, key: int):

    def remove_min_freq(self, key: int):

    def get(self, key: int) -> int:
        if not key in self.key2val:
            return -1
        increase_freq(key)
        return self.key2val(key)


    def put(self, key: int, value: int) -> None:
        if key in key2val:
            self.key2val[key] = value
            increase_freq(key)
            return
        
        if len(self.key2val) == capacity:
            remove_min_freq(key)

        self.key2val[key] = value
        self.key2freq[key] = 1
        self.freq2keys[1].append(key)



# Your LFUCache object will be instantiated and called as such:
# obj = LFUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
