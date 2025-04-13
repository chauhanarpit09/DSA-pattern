
---

# Prefix Sum / Subarray Tricks

## Pattern Breakdown

The **Prefix Sum** technique stores cumulative sums up to each index in an array. This allows you to compute the sum of any subarray in constant time (O(1)) by leveraging the difference between two prefix sums.

### Prefix Sum Technique

- **Problem**: Given an array `nums[]` of size `n`, find the sum of elements in the subarray `nums[l...r]`.
- **Solution**: Compute the prefix sum for the array, and then the sum of the subarray can be computed as:
  ```java
  int sum = prefix[r + 1] - prefix[l];
  ```
  where `prefix[i] = nums[0] + nums[1] + ... + nums[i-1]` and `prefix[0] = 0`.

### Range Sum Query
- **Problem**: Given an array, perform multiple range sum queries.
- **Example**: `nums = [1, 2, 3, 4, 5]`, calculate the sum from index `l` to `r`.
- **Solution**:
  ```java
  // Precompute the prefix sum
  int[] prefix = new int[nums.length + 1];
  for (int i = 0; i < nums.length; i++) {
      prefix[i + 1] = prefix[i] + nums[i];
  }

  // Query sum from l to r
  int rangeSum = prefix[r + 1] - prefix[l];
  return rangeSum;
  ```

---

## Problems to Practice

### 1. Subarray Sum Equals K (Leetcode 560)

#### Problem:
- Given an array of integers `nums` and an integer `k`, find the total number of continuous subarrays whose sum equals `k`.

#### Pattern:
- **Prefix Sum + Hash Map**: Maintain a hashmap to store how many times each prefix sum has occurred. The key insight is that if `prefix[j] - prefix[i] == k`, then the subarray `nums[i+1...j]` has sum `k`.

```java
int countSubarrays(int[] nums, int k) {
    Map<Integer, Integer> prefixSumCount = new HashMap<>();
    prefixSumCount.put(0, 1); // to handle sum exactly equal to k
    int sum = 0, count = 0;
    
    for (int num : nums) {
        sum += num;
        if (prefixSumCount.containsKey(sum - k)) {
            count += prefixSumCount.get(sum - k);
        }
        prefixSumCount.put(sum, prefixSumCount.getOrDefault(sum, 0) + 1);
    }
    return count;
}
```

#### Similar Problem :
- **Leetcode 560: Subarray Sum Equals K**  
  [Problem Link](https://leetcode.com/problems/subarray-sum-equals-k/)
  
- **Leetcode 523: Continuous Subarray Sum**  
  [Problem Link](https://leetcode.com/problems/continuous-subarray-sum/)
  
- **Leetcode 862: Shortest Subarray with Sum at Least K**  
  [Problem Link](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)
  
- **Leetcode 974: Subarrays Divisible by K**  
  [Problem Link](https://leetcode.com/problems/subarrays-divisible-by-k/)


---

### 2. Maximum Sum Subarray of Size K (Leetcode 209)

#### Problem:
- Given an array of integers and a number `k`, find the maximum sum of a subarray of size `k`.

#### Pattern:
- **Sliding Window + Prefix Sum**: Use a sliding window to keep track of the sum of `k` elements, and update the sum as you slide the window through the array.
  
```java
int maxSumSubarray(int[] nums, int k) {
    int windowSum = 0, maxSum = Integer.MIN_VALUE;
    for (int i = 0; i < nums.length; i++) {
        windowSum += nums[i];
        if (i >= k - 1) {
            maxSum = Math.max(maxSum, windowSum);
            windowSum -= nums[i - k + 1];
        }
    }
    return maxSum;
}
```
#### Similar Problem :

- **Leetcode 209: Minimum Size Subarray Sum**  
  [Problem Link](https://leetcode.com/problems/minimum-size-subarray-sum/)
  
- **Leetcode 567: Permutation in String**  
  [Problem Link](https://leetcode.com/problems/permutation-in-string/)
  
- **Leetcode 643: Maximum Average Subarray I**  
  [Problem Link](https://leetcode.com/problems/maximum-average-subarray-i/)
  
- **Leetcode 303: Range Sum Query - Immutable**  
  [Problem Link](https://leetcode.com/problems/range-sum-query-immutable/)

- **Leetcode 1186: Maximum Subarray Sum with One Deletion**  
  [Problem Link](https://leetcode.com/problems/maximum-subarray-sum-with-one-deletion/)


---

### 3. Subarray Product Less Than K (Leetcode 713)

#### Problem:
- Given an array of positive integers `nums[]` and an integer `k`, find the number of contiguous subarrays where the product of all the elements is less than `k`.

#### Pattern:
- **Sliding Window with Product**: Instead of sum, maintain the product of the elements inside the sliding window. Adjust the left bound of the window when the product exceeds `k`.

```java
int numSubarraysProductLessThanK(int[] nums, int k) {
    int product = 1, count = 0, left = 0;
    for (int right = 0; right < nums.length; right++) {
        product *= nums[right];
        while (product >= k && left <= right) {
            product /= nums[left++];
        }
        count += right - left + 1;
    }
    return count;
}
```

---

### 4. Find the Largest Sum of Non-Overlapping Subarrays (Leetcode 1031)

#### Problem:
- Given an array `nums` and two integers `L` and `M`, find the largest sum of elements in two non-overlapping subarrays such that one subarray has at least `L` elements, and the other has at least `M` elements.

#### Pattern:
- **Prefix Sum + Sliding Window + Dynamic Programming**: Use dynamic programming to track the largest sum of subarrays ending at each index and sliding windows to ensure the subarrays do not overlap.

```java
int maxSumTwoNoOverlap(int[] nums, int L, int M) {
    int[] prefix = new int[nums.length + 1];
    for (int i = 0; i < nums.length; i++) {
        prefix[i + 1] = prefix[i] + nums[i];
    }
    
    int maxL = 0, maxM = 0, result = 0;
    
    for (int i = L + M; i <= nums.length; i++) {
        maxL = Math.max(maxL, prefix[i - M] - prefix[i - M - L]);
        maxM = Math.max(maxM, prefix[i - L] - prefix[i - L - M]);
        result = Math.max(result, Math.max(prefix[i] - prefix[i - L] + maxM, prefix[i] - prefix[i - M] + maxL));
    }
    
    return result;
}
```


#### Similar Problem :

- **Leetcode 713: Subarray Product Less Than K**  
  [Problem Link](https://leetcode.com/problems/subarray-product-less-than-k/)
  
- **Leetcode 328: Odd Even Linked List**  
  [Problem Link](https://leetcode.com/problems/odd-even-linked-list/)
  
- **Leetcode 930: Binary Subarrays With Sum**  
  [Problem Link](https://leetcode.com/problems/binary-subarrays-with-sum/)
  
- **Leetcode 1052: Grumpy Bookstore Owner**  
  [Problem Link](https://leetcode.com/problems/grumpy-bookstore-owner/)


---

### 5. Maximum Subarray Sum with One Deletion (Leetcode 1186)

#### Problem:
- Given an integer array `arr[]`, return the maximum sum of a subarray after deleting **at most** one element.

#### Pattern:
- **Dynamic Programming**: Use two arrays to track the maximum subarray sum with or without deleting an element at each position.

```java
int maximumSum(int[] arr) {
    int n = arr.length;
    int[] dp1 = new int[n], dp2 = new int[n];
    dp1[0] = arr[0]; dp2[0] = 0;
    int result = arr[0];
    
    for (int i = 1; i < n; i++) {
        dp1[i] = Math.max(arr[i], dp1[i - 1] + arr[i]);
        dp2[i] = Math.max(dp2[i - 1] + arr[i], dp1[i - 1]);
        result = Math.max(result, Math.max(dp1[i], dp2[i]));
    }
    return result;
}
```

## Similar problems:

- **Leetcode 121: Best Time to Buy and Sell Stock**  
  [Problem Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
  
- **Leetcode 152: Maximum Product Subarray**  
  [Problem Link](https://leetcode.com/problems/maximum-product-subarray/)
  
- **Leetcode 322: Coin Change**  
  [Problem Link](https://leetcode.com/problems/coin-change/)
  
- **Leetcode 740: Delete and Earn**  
  [Problem Link](https://leetcode.com/problems/delete-and-earn/)

---

## Key Takeaways for Prefix Sum / Subarray Tricks:

- **Prefix Sum** is a powerful technique to calculate the sum of elements in a subarray in constant time, after precomputing the prefix array.
- These problems often require a **combination of DP**, **sliding windows**, and **hashmaps** for optimization.
- **Pattern to Remember**: Use **prefix sum** for any problem related to **subarray sum queries**, and **sliding window** for problems with variable length subarrays or for optimizing space-time complexity.


## Tips:
- Focus on **Prefix Sum**, **Sliding Window**, and **Hash Map** as core tools.
- Practice solving problems by recognizing these patterns in questions.
- Work on optimizing both **time complexity** and **space complexity**.
