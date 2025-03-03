# Product of Array Except Self - Example Walkthrough

This document walks through the solution to the "Product of Array Except Self" problem using the two-pass prefix/suffix approach, with a detailed example to clarify the concept.

## Problem Statement
Given an integer array `nums`, return an array `answer` where `answer[i]` is the product of all elements in `nums` except `nums[i]`. Solve it without division, in O(n) time, and with O(1) extra space (excluding the output array).

### Example
**Input:** `nums = [1,2,3,4]`  
**Expected Output:** `[24,12,8,6]`  
- `answer[0] = 2 × 3 × 4 = 24`  
- `answer[1] = 1 × 3 × 4 = 12`  
- `answer[2] = 1 × 2 × 4 = 8`  
- `answer[3] = 1 × 2 × 3 = 6`

## Solution Approach
We use a **two-pass prefix/suffix pattern**:
1. First pass: Compute products of all elements to the left of each position.
2. Second pass: Multiply by products of all elements to the right.
3. Use the output array to store intermediate results.

### Code
```python
def productExceptSelf(nums):
    n = len(nums)
    answer = [1] * n  # Initialize with 1s
    
    # First pass: Left-to-right prefix products
    left_product = 1
    for i in range(n):
        answer[i] = left_product
        left_product *= nums[i]
    
    # Second pass: Right-to-left suffix products
    right_product = 1
    for i in range(n-1, -1, -1):
        answer[i] *= right_product
        right_product *= nums[i]
    
    return answer

Step-by-Step Walkthrough
Let's walk through the example nums = [1,2,3,4] to understand how it works.

Initialization
n = 4
answer = [1,1,1,1] (start with all 1s, the multiplicative identity)
First Pass: Left-to-Right (Prefix Products)
Calculate products of all elements before each position.

Iteration Details
i = 0:
answer[0] = left_product = 1 (nothing before 1)
left_product = 1 * nums[0] = 1 * 1 = 1
answer = [1,1,1,1]

i = 1:
answer[1] = left_product = 1 (1 before 2)
left_product = 1 * nums[1] = 1 * 2 = 2
answer = [1,1,1,1]

i = 2:
answer[2] = left_product = 2 (1 × 2 before 3)
left_product = 2 * nums[2] = 2 * 3 = 6
answer = [1,1,2,1]

i = 3:
answer[3] = left_product = 6 (1 × 2 × 3 before 4)
left_product = 6 * nums[3] = 6 * 4 = 24
answer = [1,1,2,6]

Second Pass: Right-to-Left (Suffix Products)
Multiply by products of all elements after each position.

Iteration Details
i = 3:
answer[3] = answer[3] * right_product = 6 * 1 = 6 (nothing after 4)
right_product = 1 * nums[3] = 1 * 4 = 4
answer = [1,1,2,6]

i = 2:
answer[2] = answer[2] * right_product = 2 * 4 = 8 (4 after 3)
right_product = 4 * nums[2] = 4 * 3 = 12
answer = [1,1,8,6]

i = 1:
answer[1] = answer[1] * right_product = 1 * 12 = 12 (3 × 4 after 2)
right_product = 12 * nums[1] = 12 * 2 = 24
answer = [1,12,8,6]

i = 0:
answer[0] = answer[0] * right_product = 1 * 24 = 24 (2 × 3 × 4 after 1)
right_product = 24 * nums[0] = 24 * 1 = 24
answer = [24,12,8,6]

Verification

answer[0] = 1 * (2×3×4) = 24
answer[1] = 1 * (3×4) = 12
answer[2] = (1×2) * 4 = 8
answer[3] = (1×2×3) * 1 = 6

Why It Works

First Pass: Stores the product of all elements to the left in answer.
Second Pass: Multiplies each position by the product of all elements to the right.
No Division: Uses running products (left_product and right_product) instead.
O(n) Time: Two passes, each O(n).
O(1) Space: Only uses the output array and two variables.

Key Insights
Splits the problem into "before" and "after" contributions.
Uses the output array as working space.
Running products accumulate incrementally, avoiding recalculation.
