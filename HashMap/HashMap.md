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
Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.



#### Example 1:

Input: nums = [1,2,3,1]

Output: true

#### Explanation:

The element 1 occurs at the indices 0 and 3.

#### Example 2:

Input: nums = [1,2,3,4]

Output: false

#### Explanation:

All elements are distinct.

#### Example 3:

Input: nums = [1,1,1,3,3,4,3,2,4,2]

Output: true

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
Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.



#### Example 1:

Input: nums1 = [1,2,2,1], nums2 = [2,2]

Output: [2,2]
#### Example 2:

Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]

Output: [4,9]

##### Explanation: 
[9,4] is also accepted.

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

# 6. Top K Frequent Elements

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.



#### Example 1:

Input: nums = [1,1,1,2,2,3], k = 2

Output: [1,2]

#### Example 2:

Input: nums = [1], k = 1

Output: [1]

### Brute Force Approach
#### Count Frequencies:

- Use a HashMap to count the frequency of each element.
#### Sort by Frequency:
- Convert the frequency map to a list of Map.Entry and sort this list based on the frequency values.
#### Extract Top K Elements:
- Retrieve the first k elements from the sorted list.

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> frequency = new HashMap<>();

        for(int num:nums){
            frequency.put(num, frequency.getOrDefault(num, 0)+1);
        }

        List<Map.Entry<Integer, Integer>> list = new ArrayList<>(frequency.entrySet());
        list.sort((a,b) -> b.getValue() - a.getValue());

        int[] result = new int[k];

        for(int i=0;i<k;i++){
            result[i] = list.get(i).getKey();
        }

        return result;

    }
}
```

#### Explanation:

##### Count Frequencies:
- We use a HashMap to count how often each number appears. Time complexity O(N), where N is the number of elements in nums

##### Sort by Frequency:

- Convert the frequency map to a list of entries and sort this list based on the frequency values. Time complexity O(NlogN), where
  N is the number of unique elements.

##### Extract Top K:

- Extract the top K elements from the sorted list. Time complexity 𝑂(𝐾) since we are iterating through the first K elements.


Total Complexity:

#### Time Complexity:
- O(NlogN)
#### Space Complexity:
- O(N) (for storing the frequency map and the sorted list).

# 7. Happy Number

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return true if n is a happy number, and false if not.



#### Example 1:

Input: n = 19

Output: true

#### Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
#### Example 2:

Input: n = 2

Output: false

### Brute Force Approach
#### Calculate Sum of Squares:

- For a given number, calculate the sum of the squares of its digits.
#### Check for Cycles:

- Use a HashSet to keep track of the numbers that have been seen.
- If you encounter a number that you've seen before, then you are in a cycle and the number is not happy.

```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();

        while(n!=1 && !set.contains(n)){
            set.add(n);
            n = findSquare(n);
        }

        return n==1;
    }

    private int findSquare(int n){

        int square =0;
        while(n>0){
            int rem = n%10;
            square += rem*rem;
            n /= 10;
        }

        return square;
    }
}
```

### Explanation:

#### Calculate Sum of Squares:

- For each number, compute the sum of the squares of its digits.
#### Check for Cycles:

- Use a HashSet to track previously seen numbers to detect cycles.
- Time Complexity: Worst case is linear in the number of digits of the number, with a maximum of around 100 steps (cycle detection).

#### Time Complexity:
O(logN), where N is the number of digits in n.
#### Space Complexity:
O(logN), for storing seen numbers.

### Optimized Approach
The optimized approach uses the Floyd's Cycle Detection Algorithm (Tortoise and Hare algorithm) to detect cycles. This method uses two pointers to detect cycles in a sequence.

#### Use Two Pointers:

- One pointer moves one step at a time (slow pointer).
- Another pointer moves two steps at a time (fast pointer).

#### Detect Cycles:

- If there is a cycle, the two pointers will eventually meet.
```java
class Solution {
   public boolean isHappy(int n) {

      int slow = n;
      int fast = n;

      do {
         slow = findSquare(slow);
         fast = findSquare(findSquare(fast));
      } while(slow != fast);

      if(slow == 1){
         return true;
      }

      return false;
   }

   private int findSquare(int number){
      int ans = 0;
      while(number > 0){
         int rem = number%10;
         ans += rem * rem;
         number /= 10;
      }

      return ans;
   }
}
```

#### Explanation:

#### Two Pointers:

- Use one pointer to move one step at a time and another to move two steps at a time.
- If the fast pointer reaches 1, the number is happy. If the slow pointer meets the fast pointer, a cycle is detected.
#### Sum of Squares:

- Same as before, calculate the sum of squares of digits.

#### Time Complexity:
O(logN), where N is the number of digits in n.
#### Space Complexity:
O(1), since no extra space is used for cycle detection.

# 8. Find the Difference
You are given two strings s and t.

String t is generated by random shuffling string s and then add one more letter at a random position.

Return the letter that was added to t.



#### Example 1:

Input: s = "abcd", t = "abcde"

Output: "e"

#### Explanation:
'e' is the letter that was added.
#### Example 2:

Input: s = "", t = "y"

Output: "y"

### Brute Force Approach
The brute force approach involves counting the frequency of each character in both strings and comparing the counts to find the extra character in t.

#### Steps:
1. Create two arrays (or hash maps) to count the frequency of characters in s and t.
2. Compare the frequency of each character in both strings.
3. The character that has a frequency difference of 1 is the extra character.

```java
class Solution {
    public char findTheDifference(String s, String t) {
        Map<Character, Integer> mapS = new HashMap<>();
        Map<Character, Integer> mapT = new HashMap<>();

        for(char c:s.toCharArray()){
            mapS.put(c, mapS.getOrDefault(c, 0)+1);
        }

        for(char c:t.toCharArray()){
            mapT.put(c, mapT.getOrDefault(c, 0)+1);
        }

        for(char c:t.toCharArray()){
            if(!mapS.containsKey(c) || mapT.get(c) > mapS.get(c)){
                return c;
            }
        }

        return ' ';
    }
}
```

#### Explanation:

- We use two HashMaps to count the occurrences of characters in both strings.
- Then, we compare the counts in s and t. The character that appears one extra time in t is returned.
#### Time Complexity:

O(N), where N is the length of the strings (since t has one more character).
#### Space Complexity:

O(N) for storing the frequency maps.
### Optimized Approach
A more efficient approach uses bitwise XOR to find the extra character. This works because XOR of two identical characters cancels out to 0. By XORing all characters in both strings, only the extra character in t will remain.

#### Steps:
1. Initialize a variable result to 0.
2. XOR all characters in s and t together.
3. The final value of result will be the extra character.

```java
class Solution {
    public char findTheDifference(String s, String t) {
       int result=0;

       for(char c:s.toCharArray()){
            result ^= c;
       }

       for(char c:t.toCharArray()){
            result ^= c;
       }

       return (char)result;
    }
}
```

# 9. 4Sum
Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:

- 0 <= a, b, c, d < n
- a, b, c, and d are distinct.
- nums[a] + nums[b] + nums[c] + nums[d] == target

You may return the answer in any order.



#### Example 1:

Input: nums = [1,0,-1,0,-2,2], target = 0

Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
#### Example 2:

Input: nums = [2,2,2,2,2], target = 8

Output: [[2,2,2,2]]
### Brute Force
The brute force approach involves trying every combination of four numbers and checking if their sum is equal to the target. This would be done using four nested loops.

#### Steps:
1. Iterate through all combinations of four numbers in the array.
2. Check if their sum equals the target.
3. Store unique quadruplets.
#### Time Complexity:
- O(N^4) where N is the number of elements in the array.

This approach would take too long for larger inputs, so it's not practical. Hence, we move to more optimized solutions.

### Optimized Approach Using Sorting + Two-Pointer Technique
This optimized approach reduces the time complexity by using sorting and the two-pointer technique. It's similar to the 3Sum problem, but with an extra loop for the fourth element.

#### Steps:
1. Sort the array.
2. Iterate through the array using two nested loops (i and j).
3. For each pair (i, j), use the two-pointer technique to find the remaining two numbers that sum to the target - (nums[i] + nums[j]).
4. Skip duplicate quadruplets to ensure the result contains only unique sets.
#### Algorithm:
1. Sort the Array: Sorting helps in skipping duplicate numbers and applying the two-pointer technique.
2. Nested Loops: Use two loops for the first two elements of the quadruplet.
3. Two Pointers: For the remaining two elements, use two pointers (one starting from left = j + 1 and one from right = n - 1).
4. Skip Duplicates: After finding valid quadruplets, ensure that no duplicate sets are included in the final result.