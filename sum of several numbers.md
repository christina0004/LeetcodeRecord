# sum of several numbers
1. 两数之和 1

   给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

   你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

   示例:

   给定 nums = [2, 7, 11, 15], target = 9

   因为 nums[0] + nums[1] = 2 + 7 = 9
   所以返回 [0, 1]

   * Solution 1

     遍历法

   ```python
   class Solution:
       def twoSum(self, nums: List[int], target: int) -> List[int]:
            for i in range(len(nums)-1):
                for j in range(i+1,len(nums)):
                    if nums[i]+nums[j]==target:
                        return [i,j]
   ```

   * Solution 2

      字典存储

     ```python
     class Solution:
         def twoSum(self, nums, target):
             """
             :type nums: List[int]
             :type target: int
             :rtype: List[int]
             """
             hashmap = {}
             for index, num in enumerate(nums):
                 another_num = target - num
                 if another_num in hashmap:
                     return [hashmap[another_num], index]
                 hashmap[num] = index
             return None
     ```

2. 三数、四数、k数之和 15\18

   给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

   注意：

   答案中不可以包含重复的四元组。

   示例：

   给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

   满足要求的四元组集合为：
   [ [-1,  0, 0, 1],
     [-2, -1, 1, 2],
     [-2,  0, 0, 2] ]

   

   四数之和完全是三数之和的延申，故在此省略三数之和的解法。

   * Solution 1

     两层嵌套， 双指针首位缩进查找

   ```python
   class Solution:
       def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
           if len(nums)<4:
               return []
           res = []
           nums.sort()
           for i in range(len(nums)-3):
               if i>0 and nums[i]==nums[i-1]:
                   continue
               if nums[i]*4>target:
                   break
               for j in range(i+1,len(nums)-2):
                   if nums[i]+nums[j]*3>target:
                       break
                   if j>i+1 and nums[j] == nums[j-1]:
                       continue
                   m = j+1
                   n = len(nums)-1
                   while m<n:
                       if nums[i]+nums[j]+nums[m]+nums[n]==target:
                           res.append([nums[i],nums[j], nums[m] ,nums[n]])
                           m+=1
                           n-=1
                           while m<n and nums[m]==nums[m-1]:
                               m+=1
                           while m<n and nums[n]==nums[n+1]:
                               n-=1
                       elif nums[i]+nums[j]+nums[m]+nums[n]>target:
                           n-=1
                       elif nums[i]+nums[j]+nums[m]+nums[n]<target:
                           m+=1
           return res
   ```

    * Solution 2

      递归k数之和解法

   ```python
   class Solution:
       def fourSum(self, nums, target):
           def findNsum(nums, target, N, result, results):
               if len(nums) < N or N < 2 or target < nums[0]*N or target > nums[-1]*N:  # early termination
                   return
               if N == 2: # two pointers solve sorted 2-sum problem
                   l,r = 0,len(nums)-1
                   while l < r:
                       s = nums[l] + nums[r]
                       if s == target:
                           results.append(result + [nums[l], nums[r]])
                           l += 1
                           while l < r and nums[l] == nums[l-1]:
                               l += 1
                       elif s < target:
                           l += 1
                       else:
                           r -= 1
               else: # recursively reduce N
                   for i in range(len(nums)-N+1):
                       if i == 0 or (i > 0 and nums[i-1] != nums[i]):
                           findNsum(nums[i+1:], target-nums[i], N-1, result+[nums[i]], results)
   
           results = []
           findNsum(sorted(nums), target, 4, [], results)
           return results
   ```

   

3. 四数相加II 454

   给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

   为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

   输入:
   A = [ 1, 2]
   B = [-2,-1]
   C = [-1, 2]
   D = [ 0, 2]

   输出:
   2

   解释:
   两个元组如下:
   1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0

   2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0

      

   * Solution 1

   两数之和的延申，字典储存前两个数的和，后两数两层嵌套遍历查找。

   ```python
   class Solution:
       def fourSumCount(self, A: List[int], B: List[int], C: List[int], D: List[int]) -> int:
           dic_1 = collections.Counter([a+b for a in A for b in B])
           res = 0
           for i in range(len(C)):
               for j in range(len(D)):
                   res+=dic_1.get(-C[i]-D[j],0)
           return res
   ```

   

   
