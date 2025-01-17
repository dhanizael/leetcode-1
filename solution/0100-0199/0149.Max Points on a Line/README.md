# [149. 直线上最多的点数](https://leetcode-cn.com/problems/max-points-on-a-line)

[English Version](/solution/0100-0199/0149.Max%20Points%20on%20a%20Line/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>给定一个二维平面，平面上有&nbsp;<em>n&nbsp;</em>个点，求最多有多少个点在同一条直线上。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> [[1,1],[2,2],[3,3]]
<strong>输出:</strong> 3
<strong>解释:</strong>
^
|
| &nbsp; &nbsp; &nbsp; &nbsp;o
| &nbsp; &nbsp; o
| &nbsp;o &nbsp;
+-------------&gt;
0 &nbsp;1 &nbsp;2 &nbsp;3  4
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
<strong>输出:</strong> 4
<strong>解释:</strong>
^
|
|  o
| &nbsp;&nbsp;&nbsp;&nbsp;o&nbsp;&nbsp;      o
| &nbsp;&nbsp;&nbsp;&nbsp;   o
| &nbsp;o &nbsp;      o
+-------------------&gt;
0 &nbsp;1 &nbsp;2 &nbsp;3 &nbsp;4 &nbsp;5 &nbsp;6</pre>

## 解法

<!-- 这里可写通用的实现逻辑 -->

在平面上确定一个点 `points[i]`，其他点与 `point[i]` 可以求得一个斜率，斜率相同的点意味着它们与 `points[i]` 在同一条直线上。

所以可以用哈希表作为计数器，其中斜率作为 key，然后累计当前点相同的斜率出现的次数。斜率可能是小数，我们可以用分数形式表示，先求分子分母的最大公约数，然后约分，最后将“分子.分母” 作为 key 即可。

需要注意，如果平面上有和当前点重叠的点，如果进行约分，会出现除 0 的情况，那么我们单独用一个变量 duplicate 统计重复点的个数，重复点一定是过当前点的直线的。

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```python
class Solution:
    def maxPoints(self, points: List[List[int]]) -> int:
        def gcd(a, b) -> int:
            return a if b == 0 else gcd(b, a % b)
            
        n = len(points)
        if n < 3:
            return n
        res = 0
        for i in range(n - 1):
            counter = collections.Counter()
            t_max = duplicate = 0
            for j in range(i + 1, n):
                delta_x = points[i][0] - points[j][0]
                delta_y = points[i][1] - points[j][1]
                if delta_x == 0 and delta_y == 0:
                    duplicate += 1
                    continue
                g = gcd(delta_x, delta_y)
                d_x = delta_x // g
                d_y = delta_y // g
                key = f'{d_x}.{d_y}'
                counter[key] += 1
                t_max = max(t_max, counter[key])
            res = max(res, t_max + duplicate + 1)
        return res
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```java
class Solution {
    public int maxPoints(int[][] points) {
        int n = points.length;
        if (n < 3) {
            return n;
        }
        int res = 0;
        for (int i = 0; i < n - 1; ++i) {
            Map<String, Integer> kCounter = new HashMap<>();
            int max = 0;
            int duplicate = 0;
            for (int j = i + 1; j < n; ++j) {
                int deltaX = points[i][0] - points[j][0];
                int deltaY = points[i][1] - points[j][1];
                if (deltaX == 0 && deltaY == 0) {
                    ++duplicate;
                    continue;
                }
                int gcd = gcd(deltaX, deltaY);
                int dX = deltaX / gcd;
                int dY = deltaY / gcd;
                String key = dX + "." + dY;
                kCounter.put(key, kCounter.getOrDefault(key, 0) + 1);
                max = Math.max(max, kCounter.get(key));
            }
            res = Math.max(res, max + duplicate + 1);
        }
        return res;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

### **...**

```

```

<!-- tabs:end -->
