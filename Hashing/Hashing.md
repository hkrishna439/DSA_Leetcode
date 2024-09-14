# Two Sum
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.



### Example 1:

Input: nums = [2,7,11,15], target = 9

Output: [0,1]

Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

### Example 2:

Input: nums = [3,2,4], target = 6

Output: [1,2]

### Example 3:

Input: nums = [3,3], target = 6

Output: [0,1]


### Brute Force Approach

The brute force approach involves checking every possible pair of numbers in the array to see if their sum equals the target. For each element in the array, you iterate through the rest of the elements to check for a match.

#### Steps:
1. Loop through the array for each element.
2. For each element, check if any of the remaining elements add up to the target sum.
3. Return the indices of the two elements.

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                if(nums[i]+nums[j] == target){
                    return new int[]{i,j};
                }
            }
        }

        return new int[2];
    }
}
```
#### Time Complexity:
O(n²) because for every element, we check all the elements after it.

#### Space Complexity:
O(1) as no extra space is used apart from the result array.


### Optimized Approach (Hash Map) 

   The optimized approach uses a hash map (or hash table) to store the difference between the target and the current element, along with the index of the current element. As we iterate through the array, we check if the current element exists in the hash map, which would mean we have found the pair.

#### Steps:
1. Initialize an empty hash map.
2. For each element in the array:

    - Compute the difference between the target and the current element (diff = target - nums[i]).
    - Check if the difference exists in the hash map:
      - If it exists, return the current index and the index stored in the hash map.
      - If it doesn't exist, store the current element's value and index in the hash map.
3. Return the indices when the target sum is found.

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] ans = new int[2];

        Map<Integer, Integer> map = new HashMap<>();

        for(int i=0;i<nums.length;i++){
            int secNum = target - nums[i];

            if(map.containsKey(secNum)){
                ans[0]=i;
                ans[1]=map.get(secNum);

                return ans;
            }

            map.put(nums[i], i);

        }

        return ans;
    }
}

```
#### Time Complexity:
O(n) because we pass through the array once, and looking up elements in a hash map is O(1) on average.

#### Space Complexity:
O(n) because we store up to n elements in the hash map.