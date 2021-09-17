# 数据结构与算法 - 并查集


### 概念
- **并查集：** Disjoint-Set，是用于处理一些不交集的合并及查询问题的一种数据结构，支持查询、合并以及添加操作。
- **合并：** Union，简单来说，就是把有关联的节点视为一个集群，每个集群用一个**祖宗节点**来表示。

- **查询：** Find，指的是依次查找某个节点的父节点，直至祖宗节点。

- **路径压缩：** 随着节点的加入，族谱层级可能会很深，这样查找效率很低。因此可以在由下至上查询的过程中，让当前向上移动，进而降低族谱层级，这就是**路径压缩**，单词操作复杂度为`O(logN)`。

### 代码模板

```
class DisjointSet(object):
    "并查集"
    def __init__(self, n):
        """
        parent[i]表示节点i的父节点，初始化parent[i] = i，即每个节点的父节点均为自己
        rank[i]表示当前节点的后代层数，包括自己
        """
        self.parent = list(range(n))
        self.rank = [1] * n

    def get_root(self, i):
        # 路径压缩：在查找的过程中向root节点移动，以减少层数
        # 根节点的父节点是它自己
        if self.parent[i] != self.parent[self.parent[i]]:
            self.parent[i] = self.get_root(self.parent[i])
        return self.parent[i]

    def is_connected(self, i, j):
        return self.get_root(i) == self.get_root(j)

    def union(self, i, j):
        i_root = self.get_root(i)
        j_root = self.get_root(j)

        # 合并的时候，将层数少的树连接到层数多的树，以减少合并树的高度
        if self.rank[i_root] == self.rank[j_root]:
            self.parent[i_root] = j_root
            self.rank[j_root] += 1
        elif self.rank[i_root] > self.rank[j_root]:
            self.parent[j_root] = i_root
        else:
            self.parent[i_root] = j_root
 
```

### 例题 1 - 朋友圈（省份数量）
<https://leetcode-cn.com/problems/number-of-provinces/>
![省份数量](省份数量.png "省份数量")
```
class DisjointSet:
    "并查集"
    def __init__(self, n):
        """
        parent[i]表示节点i的父节点，初始化parent[i] = i，即每个节点的父节点均为自己
        rank[i]表示当前节点的后代层数，包括自己
        """
        self.parent = list(range(n))
        self.rank = [1] * n
        self.num_of_sets = n

    def get_root(self, i):
        if self.parent[i] != self.parent[self.parent[i]]:
            self.parent[i] = self.get_root(self.parent[i])
        return self.parent[i]

    def is_connected(self, i, j):
        return self.get_root(i) == self.get_root(j)

    def union(self, i, j):
        i_root = self.get_root(i)
        j_root = self.get_root(j)

        # 合并的时候，将层数少的树连接到层数多的树，以减少合并树的高度
        if self.rank[i_root] == self.rank[j_root]:
            self.parent[i_root] = j_root
            self.rank[j_root] += 1
        elif self.rank[i_root] > self.rank[j_root]:
            self.parent[j_root] = i_root
        else:
            self.parent[i_root] = j_root
        
        self.num_of_sets -= 1

    def add(self, x):
        if x not in self.parent:
            self.parent[x] = None


class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        uf = DisjointSet(len(isConnected))
        for i in range(len(isConnected)):
            for j in range(i):
                if isConnected[i][j] and not uf.is_connected(i, j):
                    uf.union(i, j)
        
        return uf.num_of_sets

```

### 例题 2 - 岛屿数量
<https://leetcode-cn.com/problems/number-of-islands/>
![岛屿数量](岛屿数量.png "岛屿数量")
```
class DisjointSet:
    "并查集"
    def __init__(self, n):
        """
        parent[i]表示节点i的父节点，初始化parent[i] = i，即每个节点的父节点均为自己
        rank[i]表示当前节点的后代层数，包括自己
        """
        self.parent = list(range(n))
        self.rank = [1] * n
        self.num_of_sets = n

    def get_root(self, i):
        if self.parent[i] != self.parent[self.parent[i]]:
            self.parent[i] = self.get_root(self.parent[i])
        return self.parent[i]

    def is_connected(self, i, j):
        return self.get_root(i) == self.get_root(j)

    def union(self, i, j):
        i_root = self.get_root(i)
        j_root = self.get_root(j)

        # 若两个节点在同一集合中，跳过
        if i_root == j_root:
            return

        # 合并的时候，将层数少的树连接到层数多的树，以减少合并树的高度
        if self.rank[i_root] == self.rank[j_root]:
            self.parent[i_root] = j_root
            self.rank[j_root] += 1
        elif self.rank[i_root] > self.rank[j_root]:
            self.parent[j_root] = i_root
        else:
            self.parent[i_root] = j_root
        
        self.num_of_sets -= 1

    def add(self, x):
        if x not in self.parent:
            self.parent[x] = None

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        uf = DisjointSet(m*n) # i*num_col + j 作为唯一标识
        for i in range(m):
            for j in range(n):
                if grid[i][j] == "0":
                    uf.num_of_sets -= 1
                    continue
                if j + 1 < n and grid[i][j + 1] == "1":
                    uf.union(i*n + j, i*n + j + 1)
                if i + 1 < m and grid[i + 1][j] == "1":
                    uf.union(i*n + j, (i + 1)*n + j)
        
        return uf.num_of_sets
```

### 例题 3 - 尽量减少恶意软件的传播
<https://leetcode-cn.com/problems/minimize-malware-spread/>
![恶意软件传播](恶意软件传播.png "恶意软件传播")
```
class DisjointSet(object):
    "并查集"
    def __init__(self, n):
        """
        parent[i]表示节点i的父节点，初始化parent[i] = i，即每个节点的父节点均为自己
        rank[i]表示当前节点的后代层数，包括自己
        """
        self.parent = list(range(n))

        # 记录每棵树高度，用于优化合并查找效率
        self.rank = [1] * n

        # 记录每棵树中的传播源数量，这题里传播源数量大于1时，减少一个源并无效
        self.num_of_source = [0] * n

        # 记录每课树的节点树，判断可能传播的节点数量
        self.num_of_node = [1] * n

    def get_root(self, i):
        if self.parent[i] != self.parent[self.parent[i]]:
            self.parent[i] = self.get_root(self.parent[i])
        return self.parent[i]

    def is_connected(self, i, j):
        return self.get_root(i) == self.get_root(j)

    def union(self, i, j):
        i_root = self.get_root(i)
        j_root = self.get_root(j)

        # 合并的时候，将层数少的树连接到层数多的树，以减少合并树的高度
        if self.rank[i_root] == self.rank[j_root]:
            self.parent[i_root] = j_root
            self.rank[j_root] += 1
        elif self.rank[i_root] > self.rank[j_root]:
            self.parent[j_root] = i_root
        else:
            self.parent[i_root] = j_root
        self.num_of_node[i_root] += 1
        self.num_of_node[j_root] += 1

class Solution:
    def minMalwareSpread(self, graph: List[List[int]], initial: List[int]) -> int:
        uf = DisjointSet(len(graph))
        for i in range(len(graph)):
            for j in range(i):
                if graph[i][j]:
                    uf.union(i, j)

        for _, v in enumerate(initial):
            uf.num_of_source[uf.get_root(v)] += 1

        # 效果相同时返回索引小的节点，用例里并非顺序排列，因此先排序
        initial.sort()
        res = initial[0]
        max_virus = 0
        for k in initial:
            root = uf.get_root(k)
            if uf.num_of_source[root] == 1 and uf.num_of_node[root] > max_virus:
                max_virus = uf.num_of_node[root]
                res = k

        return res

```
