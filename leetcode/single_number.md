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
And ```3``` will be the solution.

Thanks for bitwise operators. Now let's implement this idea by using **three** integers. There is the reasoning: the first integer should indicate those bit positions that hit by 1 $$x \equiv 0 \pmod{3}$$ times, the second integer indicates those hit by 1 $$x \equiv 1 \pmod{3}$$ times, and the third indicates those hit by 1 $$x \equiv 2 \pmod{3}$$ times.

### Solution 2

### Extension: Single Number III

### References
- [solution 1](https://www.jianshu.com/p/ae56c3133a75?utm_campaign=hugo&utm_medium=reader_share&utm_content=note&utm_source=weixin-timeline&from=timeline)
- [solution 2](https://leetcode.com/problems/single-number-ii/discuss/167343/topic)
  
- *Something interesting: In the second reference, someone mentioned Shannon's Theorem, which motivated me to review the Information Theory developed by Shannon in 1940s, even though it has nothing to do with this Leetcode problem. I left a note when I went through some notes on Information Theory, so check this out --> [channel_capacity and binary_erasure_channel](../stats-prob/channel_capacity_and_binary_erasure_channel.md)*