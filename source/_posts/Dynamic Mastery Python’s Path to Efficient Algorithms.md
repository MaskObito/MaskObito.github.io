---
title: "Dynamic Mastery: Python’s Path to Efficient Algorithms"
date: 2025-03-28 01:58:00
tags: algorithm
---

# **动态规划之道：Python高效算法之路**

**1. 如何避免 m 和 n 写反？**

**统一变量命名习惯**

- **规则**：始终将第一个输入（如 `s1`）的长度定义为 `m`，第二个输入（如 `s2`）的长度定义为 `n`。

- **示例**：

  ```python
  m = len(s1)  # s1 的长度对应行数
  n = len(s2)  # s2 的长度对应列数
  dp = [[0] * (n + 1) for _ in range(m + 1)]
  ```

  ![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

  

**明确维度含义**

- **关键点**：将 `dp[i][j]` 的含义与问题描述绑定。
  - 例如：`dp[i][j]` 表示处理 `s1` 的前 `i` 个字符和 `s2` 的前 `j` 个字符时的状态。
  - 此时，行数对应 `s1` 的长度 `m`，列数对应 `s2` 的长度 `n`。

**2. 关键区别分析**

**(1) 当元素是「不可变类型」时（如 `int`, `str`）**

两种写法**完全等价**，所有行的元素独立。
 例如，初始化一个二维整数数组时，两种写法均可安全使用。

**示例代码**：

```python
m, n = 2, 3
dp1 = [[0 for _ in range(n)] for _ in range(m)]
dp2 = [[0] * n for _ in range(m)]

# 修改 dp1[0][0]
dp1[0][0] = 1
print(dp1)  # 输出 [[1, 0, 0], [0, 0, 0]]

# 修改 dp2[0][0]
dp2[0][0] = 1
print(dp2)  # 输出 [[1, 0, 0], [0, 0, 0]]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**(2) 当元素是「可变类型」时（如 `list`, `dict`）**

两种写法**不等价**，写法二会导致行间共享引用！

```python
# 错误写法：所有行的子列表是同一个对象的引用
dp = [[[]] * 3 for _ in range(2)]
dp[0][0].append(1)
print(dp)  # 输出 [[1], [1], [1]]，所有行的第一个元素被修改


# 正确写法：每次循环生成独立子列表
dp = [[[] for _ in range(3)] for _ in range(2)]
dp[0][0].append(1)
print(dp)  # 输出 [[1], [], []], [[], [], []]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# 一.斐波那契类型:

## 1.爬楼梯

![](/images/70.jpg)

```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        #dp[i]表示爬到第i阶的方法数目
        dp=[0]*(n+1)
        dp[1]=1#爬到第1阶只有一种方法
        if n>1:
            dp[2]=2
        if n>2:
            dp[3]=3
        for i in range(3,n+1):
            dp[i]=dp[i-1]+dp[i-2]
        return dp[n]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



# 二.矩阵:

# 三.动态规划在字符串的应用:

## 1.最长回文子串



## 2.单词拆分

![](/images/139.jpg)

 定义`dp[i]`表示字符串`s`的前`i`个字符（即`s[0..i-1]`）是否可以被拆分成字典中的单词。

对于每个位置`i`，检查所有可能的起始位置`j`，若`dp[j]`为`True`且子串`s[j:i]`在字典中，则`dp[i]`设为`True`。

从`i-1`到`start`逆序检查，找到第一个可行的`j`后立即跳出循环，提升效率

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        
        if not wordDict:
            return False
        word_set = set(wordDict)  # 转为集合，快速查找
        max_len = max(len(word) for word in wordDict)  # 字典最长单词长度
        n = len(s)
        dp = [False] * (n + 1)
        dp[0] = True  # 空字符串可拆分
        
        for i in range(1, n + 1):
            start = max(0, i - max_len)  # 优化：只需检查最近max_len个位置
            # 逆序检查，找到即跳出
            for j in range(i-1, start-1, -1):
                if dp[j] and s[j:i] in word_set:
                    dp[i] = True
                    break
        return dp[n]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3.最长回文子序列

![](/images/516.jpg)

采用标准的二维动态规划解法：

- 定义`dp[i][j]`为`s[i..j]`的最长回文子序列长度。
- 当`s[i] == s[j]`时，`dp[i][j] = dp[i+1][j-1] + 2`。
- 否则，`dp[i][j] = max(dp[i+1][j], dp[i][j-1])`。
- 从后向前遍历，确保子问题已计算。

```python
class Solution(object):
    def longestPalindromeSubseq(self, s):
        n = len(s)
        dp = [[0]*n for _ in range(n)]
        for i in range(n-1, -1, -1):
            dp[i][i] = 1
            for j in range(i+1, n):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])
        return dp[0][n-1]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 4.编辑距离

![](/images/72.jpg)

核心在于定义dp数组和不相等时如何处理。

```python
class Solution(object):
    def minDistance(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        
        #dp[i][j]表示把word1的i个字母改成word2的j个字母的最少操作数
        
        m, n = len(word1), len(word2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        
        for i in range(m + 1):
            dp[i][0] = i
        for j in range(n + 1):
            dp[0][j] = j
        
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = 1 + min(dp[i-1][j],   # 删除
                                    dp[i][j-1],   # 插入
                                    dp[i-1][j-1]) # 替换
        return dp[m][n]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 5.两个字符串的最小ASCII删除和

![](/images/712.jpg)

题型和4一样，多了一个ASCII码的用法:ord(s)

```python
class Solution(object):
    def minimumDeleteSum(self, s1, s2):
        """
        :type s1: str
        :type s2: str
        :rtype: int
        """
        m,n=len(s1),len(s2)
        #dp[i][j]表示s1中i个元素和s2中j个元素相等所需删除的最小值
        dp=[[0 for _ in range(n+1)] for _ in range(m+1)]
        #dp[i][0]=i
        for i in range(1,m+1):
            dp[i][0]=dp[i-1][0]+ord(s1[i-1])
        #dp[0][j]=j
        for j in range(1,n+1):
            dp[0][j]=dp[0][j-1]+ord(s2[j-1])
        dp[0][0]=0
        for i in range(1,m+1):
            for j in range(1,n+1):
                if s1[i-1]==s2[j-1]:
                    dp[i][j]=dp[i-1][j-1]
                else:
                    dp[i][j]=min(dp[i-1][j]+ord(s1[i-1]),dp[i][j-1]+ord(s2[j-1]))
        return dp[m][n]
                
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 6.不同的子序列

![](/images/115.jpg)

状态定义回顾

`dp[i][j]` 表示：**在 `s` 的前 `i` 个字符中，能找到 `t` 的前 `j` 个字符作为子序列的匹配方式总数**。

------

两种情况的本质

当 `s[i-1] == t[j-1]` 时，我们有两种选择：

1. **选择用 `s[i-1]` 匹配 `t[j-1]`**：
   - 此时，`s` 的前 `i-1` 个字符必须匹配 `t` 的前 `j-1` 个字符，因此继承 `dp[i-1][j-1]`。
   - **物理意义**：当前字符匹配后，剩余部分的匹配方式数由前面的子问题决定。
2. **选择不用 `s[i-1]` 匹配 `t[j-1]`**：
   - 即使字符相等，也可以选择忽略 `s[i-1]`，此时 `s` 的前 `i-1` 个字符需要匹配 `t` 的前 `j` 个字符，因此继承 `dp[i-1][j]`。
   - 物理意义：保留当前字符可能在后续的匹配中被使用（如果有多个相同字符）。

------

例子解析

假设 `s = "rabb"`，`t = "rab"`，手动计算 `dp[4][3]`（即 `s` 的前4个字符匹配 `t` 的前3个字符）：

1. **字符匹配（`s[3] = "b"`，`t[2] = "b"`）**：
   - **选择匹配**：
      	需要 `s` 的前3个字符（`"rab"`）匹配 `t` 的前2个字符（`"ra"`），即 `dp[3][2] = 1`。
   - **选择不匹配**：
      	需要 `s` 的前3个字符（`"rab"`）匹配 `t` 的前3个字符（`"rab"`），即 `dp[3][3] = 0`（因为 `s` 的前3个字符长度不够）。
   - 总和：`dp[4][3] = 1 + 0 = 1`（正确匹配方式：`r a b`）。

```python
class Solution(object):
    def numDistinct(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: int
        """
        m,n=len(s),len(t)
        #dp[i][j]表示s中前i个字母与t中j个字母相同的种类数
        dp=[[0 for _ in range(n+1)] for _ in range(m+1)]
        #初始化：当t为空字符串时，所有s的前缀都有一种匹配方式
        for i in range(m+1):
            dp[i][0]=1
        for i in range(1,m+1):
            for j in range(1,n+1):
                if s[i-1]==t[j-1]:
                    dp[i][j]=dp[i-1][j-1]+dp[i-1][j]#选择是dp[i-1][j-1]，不选是dp[i-1][j]
                else:
                    dp[i][j]=dp[i-1][j]#只能不选
        return dp[m][n]
        
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# 



# 四.最长递增子序列:

# 五.最长公共子序列:

## 1.最长公共子序列

![](/images/1143.jpg)

 **贪心选择性质**：

- 若当前字符相等，选择它必然使LCS长度至少增加1。若不选择，则需依赖后续字符的匹配，但后续字符的匹配无法保证比当前选择更优（因顺序性和子问题独立性）。

```python
class Solution(object):
    def longestCommonSubsequence(self, text1, text2):
        """
        :type text1: str
        :type text2: str
        :rtype: int
        """
        m,n=len(text1),len(text2)
        dp=[[0 for _ in range(n+1)] for _ in range(m+1)]
        #dp[i][j]表示text1中的前i个和text中的前j个最大相同子序列长度
        dp[0][0]=0
        #dp=0所以下面可以不用写
        '''
        for i in range(m+1):
            dp[i][0]=0
        for j in range(n+1):
            dp[0][j]=0
        '''
        for i in range(1,m+1):
            for j in range(1,n+1):
                if text1[i-1]==text2[j-1]:
                    dp[i][j]=dp[i-1][j-1]+1
                else:
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1])
        return dp[m][n]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 2.不相交的线

![](/images/1035.jpg)

本质和1一样，如果相等只能+1。

```python
class Solution(object):
    def maxUncrossedLines(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: int
        """
        m,n=len(nums1),len(nums2)
        dp=[[0 for _ in range(n+1)] for _ in range(m+1)]
        #dp[i][j]表示nums1的前i个数字和nums2的前j个数字的最多连线

        for i in range(1,m+1):
            for j in range(1,n+1):
                if nums1[i-1]==nums2[j-1]:
                    dp[i][j]=dp[i-1][j-1]+1
                else:
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1])
        return dp[m][n]
                         
                        
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

## 3.让字符串成为回文串的最少插入次数

![](/images/1312.jpg)

此题复杂之处在于dp的含义：`dp[i][j]`表示将字符串`s`从索引`i`到`j`的子串变为回文所需的最小插入次数。利用子串构成，两重循环是子串长度L和索引i，本质一个i和L对应一个j，所以j不用循环。

```python
class Solution(object):
    def minInsertions(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        n = len(s)
        dp = [[0] * n for _ in range(n)]
        #dp[i][j]表示将字符串s从索引i到j的子串变为回文所需的最小插入次数。
        for l in range(2, n + 1):  # l是子串长度，从2到n
            for i in range(n - l + 1):
                j = i + l - 1
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1]
                else:
                    dp[i][j] = min(dp[i+1][j], dp[i][j-1]) + 1
        return dp[0][n-1]
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# 六.买卖股票的最佳时间/状态机:

# 七.动态规划在树的应用:

# 八.背包问题:

# 九.动态规划 - 一维: