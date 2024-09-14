# 1. Two Sum
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.



#### Example 1:

Input: nums = [2,7,11,15], target = 9

Output: [0,1]

Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

#### Example 2:

Input: nums = [3,2,4], target = 6

Output: [1,2]

#### Example 3:

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

#### Explanation:
- We iterate over every possible pair of indices i and j, checking if their sum equals the target.
- If a valid pair is found, the indices of these elements are returned.
- If no valid pair is found, return empty array.

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

#### Explanation:
- As we iterate through the array, for each element nums[i], we calculate the difference target - nums[i] (the number needed to complete the pair).
- If the difference is already in the hash map, we know we’ve found the two numbers that add up to the target.
- If the difference is not in the hash map, we store the current element and its index for future reference.

#### Time Complexity:
O(n) because we pass through the array once, and looking up elements in a hash map is O(1) on average.

#### Space Complexity:
O(n) because we store up to n elements in the hash map.


# 2. Group Anagrams

Given an array of strings strs, group the
anagrams
together. You can return the answer in any order.



#### Example 1:

Input: strs = ["eat","tea","tan","ate","nat","bat"]

Output: [["bat"],["nat","tan"],["ate","eat","tea"]]

#### Explanation:

- There is no string in strs that can be rearranged to form "bat".
- The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
- The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.

#### Example 2:

Input: strs = [""]

Output: [[""]]

#### Example 3:

Input: strs = ["a"]

Output: [["a"]]

### Brute Force Approach (Not Ideal)
   The brute force approach would involve comparing every string with every other string to check if they are anagrams. However, this is computationally expensive and inefficient for larger inputs due to its O(n² * klogk) complexity (where n is the number of strings, and k is the maximum length of a string).

### Optimized Approach (Using HashMap)
   An optimized approach involves using a hash map to group the anagrams. The idea is to sort each string's characters to form a key, and use this key to group anagrams together in the map. Since two strings that are anagrams will have the same sorted version, we can use this as the key.

#### Approach:
1. Create a hash map where the key is the sorted version of each string.
2. For each string in the array:
   - Sort the characters of the string.
   - Use the sorted string as a key in the hash map.
   - Add the original string to the list of values corresponding to that key.
3. Return the values (grouped anagrams) from the hash map.

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();

        for(String str: strs){
            char[] chars = str.toCharArray();
            Arrays.sort(chars);
            String sortedStr = new String(chars);

            if(!map.containsKey(sortedStr)){
                map.put(sortedStr, new ArrayList<>());
            }

            map.get(sortedStr).add(str);
        }

        return new ArrayList<>(map.values());

    }
}
```

#### Explanation:
- Sort Each String: For each string in the input array, we sort its characters. For example, "eat" becomes "aet", and "tea" also becomes "aet".
- Use HashMap: We use a hash map to group strings that have the same sorted characters (anagrams). The sorted string is the key, and the value is a list of original strings that are anagrams.
- Return Result: At the end, we return the values of the hash map, which represent the grouped anagrams.

#### Time Complexity:
O(n * klogk) where n is the number of strings and k is the maximum length of a string. Sorting each string takes O(klogk), and we do this for all n strings.

#### Space Complexity:
O(n * k) where n is the number of strings and k is the maximum length of a string (since we store all strings in the hash map).

# 3. Longest Consecutive Sequence

Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.



#### Example 1:

Input: nums = [100,4,200,1,3,2]

Output: 4

Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
#### Example 2:

Input: nums = [0,3,7,2,5,8,4,6,0,1]

Output: 9

### Brute Force (Not Efficient)
In the brute force approach, you would sort the array first and then iterate through it, counting the length of consecutive sequences. However, this solution has a time complexity of O(n log n) due to the sorting step.

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums == null || nums.length==0){
            return 0;
        }
        
        Arrays.sort(nums);

        int longestStreak=1;
        int currentStreak=1;
        for(int i=1;i<nums.length;i++){
            if(nums[i]!=nums[i-1]){
                if(nums[i] == nums[i-1]+1){
                    currentStreak++;
                } else {
                    longestStreak = Math.max(longestStreak, currentStreak);
                    currentStreak=1;
                }
            }
        }
        return Math.max(longestStreak, currentStreak);
    }
}
```

#### Explanation:
- We first sort the array, and then iterate through the sorted array while counting the length of consecutive sequences.
- Sorting guarantees that consecutive numbers will be adjacent, but sorting takes O(n log n), which is less efficient than the hash set solution.

#### Time Complexity: 
O(n log n) because of the sorting step. 
#### Space Complexity:
O(n) because we need space for the sorted array.

### Using a HashSet (Optimal Solution)
We can achieve the optimal O(n) time complexity by using a HashSet to store the elements of the array and then iterating through the array to find the start of each consecutive sequence. This ensures that each element is processed only once.

#### Steps:
1. Add all elements to a hash set.
2. Iterate over the array and check if the current number is the start of a sequence. A number is the start of a sequence if num - 1 is not present in the hash set.
3. If the current number is the start of a sequence, find the length of the consecutive sequence by checking the presence of consecutive numbers.
4. Keep track of the maximum length of consecutive sequences found.

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums == null || nums.length==0){
            return 0;
        }
        Set<Integer> set = new HashSet<>();

        for(int num: nums){
            set.add(num);
        }

        int longestStreak = 0;
        for(int num:nums){
            if(!set.contains(num-1)){
                int currentNum = num;
                int currentStreak = 1;

                while(set.contains(currentNum+1)){
                    currentNum++;
                    currentStreak++;
                }

                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}
```

#### Explanation:
- Step 1: We add all the elements of the array to a hash set to allow O(1) time complexity for lookups.
- Step 2: For each element, we check if it's the start of a consecutive sequence by seeing if num - 1 is not in the set. If it is not, we proceed to count how long the sequence extends by checking the presence of consecutive numbers (num + 1, num + 2, etc.).
- Step 3: For each consecutive sequence, we update the longest streak found.

#### Time Complexity:
O(n), where n is the number of elements in the array. This is because each element is processed only once when added to the set and when checking for sequences.

#### Space Complexity:
O(n), since we use a hash set to store all the elements.

# 4. Contains Duplicate

### Sorting the Array(Brute force)
Another approach is to sort the array and check for consecutive duplicate elements. If two consecutive elements are the same, then the array contains duplicates.

#### steps:
1. Sort the array.
2. Iterate through the sorted array, and check if any consecutive elements are the same.
3. If you find two consecutive equal elements, return true.
4. If no duplicates are found, return false.

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);

        for(int i=1;i<nums.length;i++){
            if(nums[i]== nums[i-1]){
                return true;
            }
        }

        return false;
    }
}
```
#### Explanation:
- By sorting the array first, we bring all duplicates next to each other. Then, we simply need to compare consecutive elements.
- However, sorting takes O(n log n) time, which is less efficient than the hash set approach.

#### Time Complexity:
O(n log n) due to sorting.
#### Space Complexity:
O(1) if sorting in place, or O(n) if a copy of the array is needed for sorting.


### Using HashSet (Optimal Solution)
A simple and efficient solution involves using a HashSet to store the unique elements of the array. As you iterate through the array, you check if an element already exists in the set. If it does, return true (indicating a duplicate), otherwise add the element to the set.

#### Steps:
1. Create a hash set to store the elements encountered.
2. For each element in the array:
   - If the element is already in the set, return true.
   - Otherwise, add the element to the set.
3. If no duplicates are found after processing all elements, return false.
```java
class Solution {
   public boolean containsDuplicate(int[] nums) {
      HashSet<Integer> set = new HashSet<>();
      for(int i=0; i< nums.length; i++){
         if(set.contains(nums[i])){
            return true;
         }

         set.add(nums[i]);
      }

      return false;
   }
}
```
#### Explanation:
- The HashSet allows O(1) time complexity for both insertions and lookups.
- As we iterate through the array, we check if the current number is already in the set. If it is, we return true, otherwise, we add it to the set.

#### Time Complexity:
O(n), where n is the number of elements in the array.

#### Space Complexity:
O(n), since we are storing elements in a set.

# 5. Intersection of Two Arrays II

### Sorting Both Arrays(Brute force)
Approach is to sort both arrays and use two pointers to find the common elements. This method does not require extra space but has a higher time complexity due to sorting.

#### Steps:
1. Sort both nums1 and nums2.
2. Use two pointers to iterate through both arrays and find matching elements.
3. If the elements are equal, add them to the result and move both pointers forward.
4. If the element in nums1 is smaller, move the pointer for nums1 forward, otherwise move the pointer for nums2.

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);

        int i=0, j=0, k=0;

        int[] result = new int[Math.min(nums1.length, nums2.length)];
       
        while(i<nums1.length && j< nums2.length){
            if(nums1[i] < nums2[j]){
                i++;
            } else if(nums1[i] > nums2[j]){
                j++;
            } else {
                result[k++] = nums1[i];
                i++;
                j++;
            }
        }

        return Arrays.copyOfRange(result,0,k);
    }
}
```

#### Explanation:
- We first sort both arrays, making it easier to find common elements using two pointers.
- The two pointers i and j iterate through nums1 and nums2 respectively. When a match is found, it's added to the result array.

#### Time Complexity: 
O(n log n + m log m), where n and m are the lengths of nums1 and nums2, respectively (because of sorting).

#### Space Complexity:
O(min(n, m)), as we store the result array of the smaller length.

### Using HashMap (Optimal for Counting Occurrences)
The most efficient way to solve this problem is by using a HashMap to count the occurrences of each element in one array, and then iterating through the second array to find matching elements.

#### Steps:
1. Use a hash map to store the count of each element in the first array (nums1).
2. Iterate through the second array (nums2):
   - If the element is present in the map and its count is greater than zero, add the element to the result and decrease its count in the map.
3. Return the result as an array.
```java
class Solution {
   public int[] intersect(int[] nums1, int[] nums2) {
      List<Integer> result = new ArrayList<>();
      Map<Integer, Integer> map = new HashMap<>();

      for(int i=0; i<nums1.length;i++){
         if(map.containsKey(nums1[i])){
            map.put(nums1[i], map.getOrDefault(nums1[i],0)+1);
         } else {
            map.put(nums1[i],1);
         }
      }

      for(int i=0;i<nums2.length;i++){
         if(map.containsKey(nums2[i]) && map.get(nums2[i])>0){
            result.add(nums2[i]);
            map.put(nums2[i], map.get(nums2[i])-1);
         }
      }

      return result.stream().mapToInt(num -> num).toArray();
   }
}
```

#### Explanation:
-  We count the frequency of each element in nums1 using a HashMap.
-  We iterate through nums2 and check if the element is in the map with a non-zero count. If so, we add it to the result and decrement the count in the map.
-  The result is stored in a list and then converted to an array for the final output.

#### Time Complexity:
O(n + m), where n is the length of nums1 and m is the length of nums2. We traverse both arrays once.

#### Space Complexity:
O(min(n, m)), since we store the counts of the smaller array in the hash map.