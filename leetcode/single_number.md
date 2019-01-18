## Note on Leetcode-137 Single Number II
Explore bitwise operators and their application. My friend and I spent some time on this problem. This note presents two beautiful solutions to this problem, one by my friend Alice, the other is one of the shortest yet elegant solution I found from the Leetcode Discussion.
*Sidenote: I think bitwise operations are really cool. They reveals further the rich meaning of the commonly-used base 10 integers. That's why they still blows my mind no matter how many times I've encountered them.*

### Problem statement
Given a **non-empty** array of integers, every element appears *three* times except for one, which appears exactly once. Find that single one.
**Note:**
Your algorithm should have a linear runtime complexity and constant space complexity.
**Example 1:**
```
Input: [2, 2, 3, 2]
Output: 3
```
**Example 2:**
```
Input: [0, 1, 0, 1, 0, 1, 99]
Output: 99
```

### Solution 1
Alice's idea is as the following two parts: First, as we scan through the array, count the appearances of 1 in each bit position. For instance:
```
[4, 4, 3, 4] == base 2 ==> [100, 100, 011, 100]
Scan the array from left to right.
3 1's show up at the first position, 1 at the second, and 1 at the third.
```
Then, once we get the counter of each position, set those counters whose ```mod 3``` is 1 to be 1 and the rest are all set to 0.  
In the example above, the first position has counter 3 and 3 = 0 (mod 3), while the last two positions have counter 1 and 1 = 1 (mod 3). Hence:
```
311 ==> 011 == base 10 ==> 3
```
And `3` will be the solution.

Thanks for bitwise operators. Now let's implement this idea by using **three** integers. There is the reasoning: the first integer should indicate those bit positions that hit by 1 X = 0 (mod 3) times, the second integer indicates those hit by 1 X = 1 (mod 3) times, and the third indicates those hit by 1 X = 2 (mod 3) times. 

In fact, for each bit position, we use 3 bits to encode its condition:

```
x0 x1 x2
1  0  0 ----> 1 shows up X = 0 (mod 3) times
0  1  0 ----> 1 shows up X = 1 (mod 3) times
0  0  1 ----> 1 shows up X = 2 (mod 3) times
```

Indeed, it suffices to consider only one bit since the rest of them will do the similar thing in *parallel*:
```
[1, 1, 0, 1]
initialize x0, x1, x2 = 0, 1, 0
Scan the array from left to right:

     [1, 1, 0, 1]  [1, 1, 0, 1]  [1, 1, 0, 1]  [1, 1, 0, 1]
      ^                ^                ^                ^
x0        0             0              0            1
x1        1             0              0            0 
x2        0             1              1            0  
```
Note that base 10 form of ```x1``` is the final answer.
Here is the code:
```
def singleNumber(nums):
    x0, x1, x2 = 0, nums[0], 0
    for n in nums[1:]:
        x0 = x2 & n # mark the bits that showed up in both x2 and n
        x2 ^= x0 # bits marked by x2 are now marked by x0 by the line above, so unmark them
        x2 |= x1 & n # use the same logic to update x2. But why 'or'? See explanation below
        x1 ^= n 
        x1 ^= x0 
    return x1
```
Note that `x2 |= x1 & n`. Consider one bit position and 1 has already shown up twice(so now x2 at this bit position is 1), but the third 1 doesn't show up in the next new number. Without `|`, x2 will back to 0 since now x1 and n are both 0 at this position. For more concrete example, consider a input `[1, 0, 1, 0, 1, 0, 3]`.

The last two lines of the iteration seems a *bit* off, but it actually utilize the fact that any bit position can only have one mark, from one of x0, x1, and x2. 

Thank you Alice.

### Solution 2
The second solution applies the similar idea as the first one, except that it tries to encode the three condisions of each bit by two other bits, while the first solution use three bits for each bit. Specifically, for each bit:
```
a b
1 0 ----> 1 shows up X = 1 (mod 3) times
0 1 ----> 1 shows up X = 2 (mod 3) times
0 0 ----> 1 shows up X = 3 (mod 3) times
```

Here is the code:
```
def singleNumber(nums):
    a, b = 0, 0
    for n in nums:
        a = a ^ n & ~b
        b = b ^ n & ~a
    return a
```

### Extension: Single Number III
Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

Example:
```
Input:  [1,2,1,3,2,5]
Output: [3,5]
```

TO BE UPDATED

### References
- [solution 1](https://www.jianshu.com/p/ae56c3133a75?utm_campaign=hugo&utm_medium=reader_share&utm_content=note&utm_source=weixin-timeline&from=timeline)
- [solution 2](https://leetcode.com/problems/single-number-ii/discuss/167343/topic)
  
- *Something interesting: In the second reference, someone mentioned Shannon's Theorem, which motivated me to review the Information Theory developed by Shannon in 1940s, even though it has nothing to do with this Leetcode problem. I left a note when I went through some notes on Information Theory, so check this out --> [channel_capacity and binary_erasure_channel](../stats-prob/channel_capacity_and_binary_erasure_channel.ipynb)*