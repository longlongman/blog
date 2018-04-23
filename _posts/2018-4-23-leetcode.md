---
layout: post
title: leetcode刷题（一）
date: 2018-4-23 22:45:00
category: "leetcode"
---
<h2>leetcode刷题记录</h2>
<p>刷刷更健康，尽量做到详细解释</p>

<h2>575 分糖果 简单</h2>
<p>Given an integer array with even length, where different numbers in this array represent different kinds of candies. Each number means one candy of the corresponding kind. You need to distribute these candies equally in number to brother and sister. Return the maximum number of kinds of candies the sister could gain.</p>
<p>Example 1:</p>
```
Input: candies = [1,1,2,2,3,3]
Output: 3
Explanation:There are three different kinds of candies (1, 2 and 3), and two candies for each kind.
Optimal distribution: The sister has candies [1,2,3] and the brother has candies [1,2,3], too. The sister has three different kinds of candies.
```
<p>Example 2:</p>
```
Input: candies = [1,1,2,3]
Output: 2
Explanation: For example, the sister has candies [2,3] and the brother has candies [1,1]. The sister has two different kinds of candies, the brother has only one kind of candies.
```
<p>
Note:
1、The length of the given array is in range [2, 10,000], and will be even.
2、The number in given array is in range [-100,000, 100,000].</p>

```
class Solution(object):
    def distributeCandies(self, candies):
        """
        :type candies: List[int]
        :rtype: int
        """
        len_ = len(candies)
        candies_num_for_each = len_ / 2
        lim = 0
        sister = {}
        for i in candies:
            if lim >= candies_num_for_each:
                break
            elif i not in sister:
                sister[i] = i
                lim = lim + 1
        return len(sister)
```
<p>很明显的贪心算法，要求姐姐最多可以有多少种不同的糖果，可以这样做，遍历所有的糖果，如果姐姐没有某类糖果，则给姐姐，注意题目要求姐弟两人得到的糖果的数量一样，则姐姐最多只能拿到一半的糖果，姐姐最多可以拿到的糖果的种类的上限也是糖果总数的一半。</p>

<h2>636 Exclusive Time of Functions 中等</h2>
<p>Given the running logs of n functions that are executed in a nonpreemptive single threaded CPU, find the exclusive time of these functions.</p>
<p>Each function has a unique id, start from 0 to n-1. A function may be called recursively or by another function.</p>
<p>A log is a string has this format : function_id:start_or_end:timestamp. For example, "0:start:0" means function 0 starts from the very beginning of time 0. "0:end:0" means function 0 ends to the very end of time 0.</p>
<p>Exclusive time of a function is defined as the time spent within this function, the time spent by calling other functions should not be considered as this function's exclusive time. You should return the exclusive time of each function sorted by their function id.</p>
<p>Example 1:</p>
```
Input:
n = 2
logs = 
["0:start:0",
 "1:start:2",
 "1:end:5",
 "0:end:6"]
Output:[3, 4]
Explanation:
Function 0 starts at time 0, then it executes 2 units of time and reaches the end of time 1. 
Now function 0 calls function 1, function 1 starts at time 2, executes 4 units of time and end at time 5.
Function 0 is running again at time 6, and also end at the time 6, thus executes 1 unit of time. 
So function 0 totally execute 2 + 1 = 3 units of time, and function 1 totally execute 4 units of time.
```
<p>
Note:
1、Input logs will be sorted by timestamp, NOT log id.
2、Your output should be sorted by function id, which means the 0th element of your output corresponds to the exclusive time of function 0.
3、Two functions won't start or end at the same time.
4、Functions could be called recursively, and will always end.
5、1 <= n <= 100
</p>

```
class Solution(object):
    def exclusiveTime(self, n, logs):
        """
        :type n: int
        :type logs: List[str]
        :rtype: List[int]
        """
        stack = []
        result = [0] * n
        pretime = 0
        for i in logs:
            temp = i.split(":")
            idx = int(temp[0])
            ftype = temp[1]
            time = int(temp[2])
            
            if len(stack) != 0:
                temp = stack[-1]
                result[temp] += time - pretime
            
            pretime = time
            
            if ftype == "start":
                stack.append(idx)
                
            else:
                temp = stack.pop()
                result[temp] += 1
                pretime += 1
        return result
```
<p>十分明显的堆栈类题目，函数的start对应着压栈，end对应着出栈，栈顶永远是当前正在运行的函数，当一个新函数start时，当前函数会暂停执行，会累计执行时间，当得到一条end记录时，一定是当前正在运行的函数结束了，栈顶应该pop。</p>

<h2>31 下一个排列 中等</h2>
<p>Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.</p>
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

```
class Solution(object):
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        if nums is None or len(nums) <= 1:
            return

        pos = None
        p = len(nums) - 2
        # find the first number that is not in correct order
        while p >= 0:
            if nums[p + 1] > nums[p]:
                pos = p
                break
            p -= 1

        if pos is None:
            self.reverse(nums, 0, len(nums) - 1)
            return

        # find the min value in the rest of the array
        minPos, minV = pos + 1, nums[pos + 1]
        for i in xrange(pos + 1, len(nums)):
           if nums[i] <= minV and nums[i] > nums[pos]:
               minV = nums[i]
               minPos = i
        # swap the two above number and reverse the array from `pos`
        nums[pos], nums[minPos] = nums[minPos], nums[pos]
        self.reverse(nums, pos + 1, len(nums) - 1)

    def reverse(self, nums, start, end):
        while start < end:
            nums[start], nums[end] = nums[end], nums[start]
            start += 1
            end -= 1
```
<p>从后往前看，若一直是升序的，则说明这个数已经是最大的了，没有下一个，任何改动都只会让数字变小，此时只用逆序排列即可，若在其中发现了降序关系，这说明这个数不是最大的，至少还有一个数比它大，从出现降序关系的两个数字的后者开始到nums的结束，寻找一个最小的大于前者的数字与前者交换，最后再将从后者到nums最后的升序子数组逆序为降序即可，此时即为所求。</p>