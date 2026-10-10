LC2333 - Minimum Sum of Squared Difference
==========================================

PROBLEM
Given two arrays nums1 and nums2 of length n, the sum of squared
difference is the sum of (nums1[i] - nums2[i])^2. You may change any
element of nums1 by +1 or -1 at most k1 times in total, and any element
of nums2 by +1 or -1 at most k2 times in total. Return the minimum
possible sum of squared difference.

CONSTRAINTS
- 1 <= n <= 10^5
- 0 <= nums1[i], nums2[i] <= 10^5
- 0 <= k1, k2 <= 10^9

KEY IDEA
Changing an element of either array changes |nums1[i] - nums2[i]| by 1.
So k1 and k2 merge into one budget: k = k1 + k2. Let d[i] be the
absolute difference at index i. The task becomes: use at most k
decrements (each floored at 0) on the d[i] values to minimize the sum
of d[i]^2.

APPROACH
Greedy: always reduce the largest difference, because lowering a value
v to v-1 saves 2v-1, which is bigger for bigger v.
Doing this one step at a time is too slow, so it is done in bulk:

1. If sum(d) <= k, return 0.
2. Count how many differences have each value (cnt[v]).
3. Sweep v from max(d) down to 1, keeping a carry of elements currently
   at level v (original ones plus those already lowered).
   - If k >= carry: lower all of them to v-1 and subtract carry from k.
   - Otherwise: k of them drop to v-1, the rest stay at v. Stop.
4. Return the sum of v^2 * cnt[v] over the final distribution.

COMPLEXITY
Time:  O(n + max(d))
Space: O(max(d))

EXAMPLE
nums1 = [1,4,10,12], nums2 = [5,8,6,9], k1 = 1, k2 = 1
d = [4,4,4,3], k = 2
Two of the 4s drop to 3, giving one 4 and three 3s: 16 + 27 = 43.

FILES
min_sum_square_diff.py   Solution.minSumSquareDiff

USAGE
    from min_sum_square_diff import Solution
    Solution().minSumSquareDiff([1,4,10,12], [5,8,6,9], 1, 1)  # 43