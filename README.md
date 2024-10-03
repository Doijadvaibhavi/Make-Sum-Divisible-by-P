# Make-Sum-Divisible-by-P

Given an array of positive integers nums, remove the smallest subarray (possibly empty) such that the sum of the remaining elements is divisible by p. It is not allowed to remove the whole array.

Return the length of the smallest subarray that you need to remove, or -1 if it's impossible.

A subarray is defined as a contiguous block of elements in the array.

 

Example 1:

Input: nums = [3,1,4,2], p = 6
Output: 1
Explanation: The sum of the elements in nums is 10, which is not divisible by 6. We can remove the subarray [4], and the sum of the remaining elements is 6, which is divisible by 6.
Example 2:

Input: nums = [6,3,5,2], p = 9
Output: 2
Explanation: We cannot remove a single element to get a sum divisible by 9. The best way is to remove the subarray [5,2], leaving us with [6,3] with sum 9.
Example 3:

Input: nums = [1,2,3], p = 3
Output: 0
Explanation: Here the sum is 6. which is already divisible by 3. Thus we do not need to remove anything.
 

Constraints:

1 <= nums.length <= 105

1 <= nums[i] <= 109

1 <= p <= 109

# SOLUTION

Intuition
The problem asks us to remove the smallest subarray such that the sum of the remaining elements is divisible by a given integer p. Our key observation is that the total sum of the array determines how much we need to adjust. We want to remove a subarray whose sum has a specific remainder when divided by p.

If we can efficiently find a subarray whose sum mod p equals the needed remainder, we can solve the problem optimally.

Approach
Total Sum Calculation: First, calculate the total sum of the array and its remainder when divided by p.
Target Subarray: We need to remove a subarray such that the sum of the remaining elements is divisible by p. The sum of the removed subarray should have the same remainder as the total sum mod p.
Prefix Sums: We utilize prefix sums to track the sum of subarrays. By keeping track of the prefix sums mod p, we can check if a subarray that satisfies the condition can be found.
Optimization with Hashmap: We use a hashmap to store the prefix sum modulo values, which helps in quickly finding a valid subarray by comparing remainders.
Complexity
Time complexity:
O(n), where (n) is the number of elements in the nums array. We only need to traverse the array once, and each operation (including the hashmap lookup) takes constant time.

Space complexity:
O(n) because we store prefix sums in a hashmap, and in the worst case, we might store up to (n) entries.

# JAVA CODE 

import java.util.HashMap;

class Solution {

    public int minSubarray(int[] nums, int p) {
    
        long totalSum = 0;
        
        for (int num : nums) {
        
            totalSum += num;
            
        }

        // Find remainder when total sum is divided by p
        
        int rem = (int)(totalSum % p);
        
        if (rem == 0) return 0; // If remainder is 0, no subarray needs to be removed

        HashMap<Integer, Integer> prefixMod = new HashMap<>();
        
        prefixMod.put(0, -1);  // Initialize to handle full prefix
        
        long prefixSum = 0;
        
        int minLength = nums.length;

        for (int i = 0; i < nums.length; ++i) {
        
            prefixSum += nums[i];
            
            int currentMod = (int)(prefixSum % p);
            
            int targetMod = (currentMod - rem + p) % p;

            if (prefixMod.containsKey(targetMod)) {
            
                minLength = Math.min(minLength, i - prefixMod.get(targetMod));
                
            }

            prefixMod.put(currentMod, i);
            
        }

        return minLength == nums.length ? -1 : minLength;
        
    }
    
}

Step-by-Step Detailed Explanation
Total Sum Calculation:

The total sum of the array is calculated to determine the remainder when divided by p.
Example: For nums = [3, 1, 4, 2], the total sum is 10, and 10 % 6 = 4.
Check if No Subarray is Needed:

If the remainder is 0, no subarray needs to be removed, and we return 0 immediately.
Prefix Sum with Modulo:

As we iterate over the array, we calculate the prefix sum modulo p for each index.
We store these modulo values in a hashmap for quick lookup. This helps us check if a previous prefix sum matches the required condition.
Efficient Subarray Removal:

For each element, we check if there exists a prefix sum modulo p that, when combined with the current prefix sum, would remove a subarray whose sum has the same remainder as rem.
The hashmap allows us to do this efficiently by storing the indices of the prefix sums.
Return Result:

After the loop, we return the minimum length of the subarray found, or -1 if no valid subarray exists.
Visualization
Let’s take Example 1:

nums = [3, 1, 4, 2], p = 6
Total Sum Calculation:
First, we calculate the sum of the entire array:

3 + 1 + 4 + 2 = 10
Remainder Check:
Find the remainder when the total sum is divided by p:

10 % 6 = 4
Since the remainder is 4, the total sum is not divisible by 6. This means we need to remove a subarray that will adjust this remainder to 0.

Target Remainder for Subarray:
We want to remove a subarray whose sum mod 6 equals 4. This will leave a sum divisible by 6 for the remaining elements.

Subarray Visualization:
One possible subarray we can remove is [4]. Removing this gives us the remaining elements [3, 1, 2]:

3 + 1 + 2 = 6
Now, 6 % 6 = 0, which satisfies the condition.

Optimal Subarray:
The subarray [4] is of length 1, which is the smallest subarray we can remove.

Here’s a visual representation of this approach:

Initial Array: [3, 1, 4, 2]
Total Sum: 10
Remainder (10 % 6): 4
Target Subarray Sum (mod 6 = 4): [4]
Remaining Elements after Removal: [3, 1, 2]
Remaining Sum: 6
Result: Length of Subarray to Remove = 1
