```python
"""
地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

示例 1：

输入：m = 2, n = 3, k = 1
输出：3
示例 2：

输入：m = 3, n = 1, k = 0
输出：1
提示：

1 <= n,m <= 100
0 <= k <= 20
"""


class Solution:
    def movingCount(self, m: int, n: int, k: int) -> int:
        """
        深度优先搜索
        定义 深度优先搜索函数
            如果当前位置超出边界 or 数位和超界 or 已访问过：
                return 0
            将该位置标记为已被访问
            递归地向下方和右方搜索，其中数位和是通过增量和公式得出，返回1+左方所有可达解数+右方可达解数
        初始化：已访问过的位置，调用深度优先搜索函数
        """
        def dfs(i, j, si, sj):
            if (i,j) in visited or si+sj>k or i >= m or j>=n:
                return 0
            visited.add((i,j))
            return 1 + dfs(i+1, j, si+1 if (i+1)%10 else si-8, sj) 
        			+ dfs(i, j+1, si, sj+1 if (j+1)%10 else sj-8)
        visited = set()
        return dfs(0, 0, 0, 0)
```

```python
class Solution:
    def movingCount(self, m: int, n: int, k: int) -> int:
        """
        广度优先搜索
        初始化：将(0,0)位置入队列，初始化可达解数为0
        while(直到队列为空):
            弹出队列头部位置
            如果当前位置超出边界 or 数位和超界：continue
            可达解数加一
            将该位置的右方和下方的位置加入队列
        返回可达解数
        """
        queue, visited = collections.deque(), set()
        queue.append((0, 0, 0, 0))
        res = 0
        while(queue):
            i,j,si,sj = queue.popleft()
            if i >= m or j>=n or si+sj>k or (i,j) in visited: continue
            visited.add((i,j))
            res += 1
            queue.append((i+1, j, si+1 if (i+1)%10 else si-8, sj))
            queue.append((i, j+1, si, sj+1 if (j+1)%10 else sj-8))
        return res
```

