# Slide Window

1. 无重复字符的最长子串 3

   给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

   示例 1:

   输入: "abcabcbb"
   输出: 3 
   解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

   * Solution 1

   维护字典和滑窗

   ```python
   class Solution:
       def lengthOfLongestSubstring(self, s: str) -> int:
           if not s:
               return 0
           cur = s[0]
           res = 1
           dic = collections.defaultdict(lambda:-1)
           dic[s[0]] = 0
           l = 0
           r = 1
           for i in range(1,len(s)):
               l = max(l,dic[s[i]]+1)
               r += 1
               dic[s[i]] = i
               res = max(res,r-l)
           return res 
   ```

2. K 个不同整数的子数组 992

   给定一个正整数数组 A，如果 A 的某个子数组中不同整数的个数恰好为 K，则称 A 的这个连续、不一定独立的子数组为好子数组。（例如，[1,2,3,1,2] 中有 3 个不同的整数：1，2，以及 3。）

   返回 A 中好子数组的数目.

   示例 1：

   输出：A = [1,2,1,2,3], K = 2
   输入：7
   解释：恰好由 2 个不同整数组成的子数组：[1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].

   * Solution 1

   维护两个滑窗（官方）

   ```python
   class Window:
       def __init__(self):
           self.count = collections.Counter()
           self.nonzero = 0
   
       def add(self, x):
           self.count[x] += 1
           if self.count[x] == 1:
               self.nonzero += 1
   
       def remove(self, x):
           self.count[x] -= 1
           if self.count[x] == 0:
               self.nonzero -= 1
   
   class Solution(object):
       def subarraysWithKDistinct(self, A, K):
           window1 = Window()
           window2 = Window()
           ans = left1 = left2 = 0
   
           for right, x in enumerate(A):
               window1.add(x)
               window2.add(x)
   
               while window1.nonzero > K:
                   window1.remove(A[left1])
                   left1 += 1
   
               while window2.nonzero >= K:
                   window2.remove(A[left2])
                   left2 += 1
   
               ans += left2 - left1
   
           return ans
   ```

   * Solution 2

     左指针移动时 tmp++

     ```python
     from collections import defaultdict
     class Solution:
         def subarraysWithKDistinct(self, A: List[int], K: int) -> int:
             # 滑动窗口
             # base case 
             if A is None or len(A) < K:
                 return 0
             hash = defaultdict(int)
             n = len(A)
             left = 0
             res = 0
             cnt = 0
             tmp = 1
             for i in range(n):
                 hash[A[i]] += 1
                 if hash[A[i]] == 1:
                     cnt += 1
                 # 移动左指针的条件，直到满足条件，这里需要注意的是左指针移动一次，算新来一个数!这个时候才tmp++,否则 tmp=1
                 while hash[A[left]] > 1 or cnt > K:
                     if cnt > K:
                         tmp = 1
                         cnt -= 1
                     else:
                         tmp += 1
                     hash[A[left]] -= 1
                     left += 1
                 if cnt == K:
                     res += tmp
             return res
     ```

     

