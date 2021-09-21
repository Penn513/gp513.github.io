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
# 理解题意后发现会有三种查询关系，那索性建立三张表
# 先理出put和get接口框架，再补充increace_freq及remove_min_freq的细节
class LFUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.key2val = {}
        self.key2freq = {}
        self.freq2keys = collections.defaultdict(collections.deque)
        self.min_freq = 1

    def increase_freq(self, key: int):
        freq = self.key2freq[key]
        self.freq2keys[freq].remove(key)
        self.freq2keys[freq + 1].append(key)
        self.key2freq[key] = freq + 1

        # update min_freq
        if freq == self.min_freq and len(self.freq2keys[freq]) == 0:
            self.min_freq = freq + 1

    def remove_min_freq(self):
        key = self.freq2keys[self.min_freq].popleft()
        self.key2val.pop(key)
        self.key2freq.pop(key)

        # update min_freq
        if len(self.freq2keys[self.min_freq]) == 0:
            i = 0
            while(i <= len(self.key2val)):
                if not len(self.freq2keys[i]) == 0:
                    self.min_freq = i
                    break
                i += 1


    def get(self, key: int) -> int:
        if self.capacity <= 0:
            return -1
            
        if not key in self.key2val:
            return -1
        self.increase_freq(key)
        return self.key2val[key]


    def put(self, key: int, value: int) -> None:
        if self.capacity <= 0:
            return

        if key in self.key2val:
            self.key2val[key] = value
            self.increase_freq(key)
            return
        
        if len(self.key2val) == self.capacity:
            self.remove_min_freq()

        # insert new key
        self.key2val[key] = value
        self.key2freq[key] = 1
        self.freq2keys[1].append(key)
        if self.min_freq > 1:
            self.min_freq = 1
```
