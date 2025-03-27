---
title: "algorithm"
date: 2025-03-28 02:53:00
tags: algorithm
---

# 1

![](/images/53.jpg)

可以根据头的大小每次更新新的数组，轻松解决

```python
class Solution(object):
    def maxSubArray(self, nums):
        max_sum = current_sum = nums[0]
        for num in nums[1:]:
            current_sum = max(num, current_sum + num)#看看要不要开启新的数组
            max_sum = max(max_sum, current_sum)
        return max_sum
```

## 发布图片顺序：

截图复制，粘贴到Typora任意地方，打开C:\Users\75192\AppData\Roaming\Typora\typora-user-images，把图片复制到D:\桌面\blog\source\images，重命名格式改为jpg，复制模板/images/53.jpg，到文章直接插入图片即可（如果不行输入路径/images/53.jpg即可）。

