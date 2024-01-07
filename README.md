# An interesting way to solve Set Mismatch
## Problem
[Leetcode 645. Set Mismatch](https://leetcode.com/problems/set-mismatch/)

You have a set of integers `s`, which originally contains all the numbers from `1` to `n`. Unfortunately, due to some error, one of the numbers in `s` got duplicated to another number in the set, which results in **repetition of one** number and **loss of another** number.

You are given an integer array `nums` representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return *them in the form of an array*.

**Example 1:** 
> <pre><b>Input:</b> nums = [1,2,2,4]<b>
> Output:</b> [2,3]</pre>

**Example 2:** 
> <pre><b>Input:</b> nums = [1,1]<b>
> Output:</b> [1,2]</pre>

**Constraints:**

- 2 <= nums.length <= 10<sup>4</sup>
- 1 <= nums[i] <= 10<sup>4</sup>


## Motivation
The most normal way to solve this problem is to record the occurence of each number (count array or hashmap), see which has a count of 2 and which has a count of 0. The code runs in linear time and uses linear space for a count array or a table. I will use a different example from Leetcode's here.

```Java
nums = [1,2,2,3,4,5,7] //order is not important
nums = [1,2,3,4,5,2,7] // looks better for explainng
// index:   0,1,2,3,4,5,6,7
// count = [0,1,2,1,1,1,0,1] 
// returns [2,6]
```

I first did the same way on Leetcode but then I saw a comment in discussion talking about using the sum of numbers and the sum of squares to solve it. I remember they posted their code but I clicked right away and said to myself, okay I'mma try to derive the equation myself. So I wish this isn't  something copying other's work, but rather a record of myself problem solving.

This brought back all the Discrete Math/Mathematics for Computer Science memories, sitting and solving problems with a pen and paper.

> :warning: **Disclaimer: this is not mathematically rigorous**


## Deriving the equation/solution
Let's call the lost number 6 as $l$, and the repeated number 2 as $r$.

Denote the sum of natural numbers from 1 to n as $s_0$/`s0` and the actual sum of numbers in the array as $s$/`s`. $s_0$ can be computed with the formula 

$$
	\sum_{i=1}^n i = \frac{n(n+1)}{2}.
$$

Similarly, denote the sum of squares from 1 to n as $sq_0$/`sq0` and the actual sum of squares $sq$/`sq`. $sq_0$ can be computed with the formula 

$$
	\sum_{i=1}^n i^2 = \frac{n(n+1)(2n+1)}{6}.
$$

We can actually extract $l$ and $r$ from the given array and represent the difference of these two numbers with terms computed above.

$$
\begin{aligned}
 (1+2+...+7)-6+2 &=(1+2+3+4+5+2+7)\\
6-2&=(1+2+...+7)-(1+2+3+4+5+2+7) .
\end{aligned}
$$

In general,

$$
\begin{equation}
	l-r=s_0-s, \tag{1}
\end{equation}
$$

which will be stored in a `delta_s` variable in the code.

Similarly,

$$
\begin{aligned}
(1^2+2^2+3^2+4^2+5^2+2^2+7^2) &= (1^2+2^2+...+7^2) - 6^2+2^2\\
6^2-2^2&=(1^2+2^2+...+7^2)-(1^2+2^2+3^2+4^2+5^2+2^2+7^2).
\end{aligned}
$$

In general,

$$
\begin{aligned}
	(l-r)(l+r)=sq_0-sq.
\end{aligned}
$$

Since the term $l-r$ is in $(1)$, we can get

$$
\begin{equation}
	l+r = \frac {sq_0-sq}{s_0-s}. \tag{2}
\end{equation}
$$

Now it's just grade 8(?) math, computing $l$ first,

$$
\begin{aligned}
	l-r + l+r &= s_0-s+\frac {sq_0-sq}{s_0-s}\\
	2l &= s_0-s+\frac {sq_0-sq}{s_0-s}\\
	l &= \frac{1}{2}(s_0-s+\frac {sq_0-sq}{s_0-s}).
\end{aligned}
$$

In code I will just use `res[1]` and `delta_s` but $r$ can be expressed with only the four computed terms.

$$
\begin{aligned}
	l-r  &= s_0-s\\
	r &= l-(s_0-s)\\
	r &= \frac{1}{2}(s_0-s+\frac {sq_0-sq}{s_0-s})-(s_0-s)\\
	 &= \frac{1}{2}(\frac {sq_0-sq}{s_0-s}+s_0-s)
\end{aligned}
$$


## Time complexity
Both `s_0` and `sq_0` can be computed in O(1) time, while `s` and `sq` needs O(n) time.
## Space complexity
O(1)

