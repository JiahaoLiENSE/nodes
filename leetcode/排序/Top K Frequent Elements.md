### 347. Top K Frequent Elements

#### Description
`Given a non-empty array of integers, return the k most frequent elements.`  

#### Example
Input: `nums = [1,1,1,2,2,3], k = 2`  
Output: `[1,2]`  

Input: `nums = [1], k = 1`  
Output: `[1]`  

#### Java
```java
/*
    The idea is to find the frequency of each number
    Group them by frequency
    return the K frequent element
    exemple: [5,5,5,4,7,3,3,7,9,6,6,3]
    5[3], 4[1], 7[2], 3[3], 9[1], 6[2]
    Grouped by frequency we have: 3[5, 3], 1[4, 9], 2[7, 6]
    By looking at the table of frequencies we see that
    the k(2) most frequent elements are 5 and 3
    the k(3) most frequent elements are 5, 3 and 7
    the k(4) most frequent elements are 5, 3, 7 and 6
    ....
    time: O(n)
    space: O(n)
*/
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        if(nums.length == k) {
            return nums;
        }
        
        // Counts the occurrences of each integer
        Map<Integer, Integer> frequencyForNum = new HashMap<>();
        for(int num : nums) {
            frequencyForNum.put(num, frequencyForNum.getOrDefault(num, 0) + 1);
        }
        
        // Group the elements by frequencies: 5[3], 4[3], 3[2] ...
        // nums.length + 1 to store frequencies index
        List<Integer>[] buckets = new ArrayList[nums.length + 1];
        for(int key : frequencyForNum.keySet()) {
            int frequency = frequencyForNum.get(key);
            if(buckets[frequency] == null) {
                buckets[frequency] = new ArrayList<>();
            }
            buckets[frequency].add(key);
        }
        
        // Group the same frequency with elements from largest to lowest: 3[5,4], 2[3], ...
        List<Integer> topK = new ArrayList<>();
        for(int i = buckets.length - 1; i >= 0 && topK.size() < k; i--) {
            if(buckets[i] == null) {
                continue;
            }
            if(buckets[i].size() <= (k - topK.size())) {
                topK.addAll(buckets[i]);
            } else {
                topK.addAll(buckets[i].subList(0, k - topK.size()));
            }
        }
        
        // Get the K most frequent element from above sorted topK
        int [] result = new int[k];
        for(int i = 0; i < k; i++) {
            result[i] = topK.get(i);
        }
        return result;
    }
}
```

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        frequencyForNum = {}
        frequencyList = {}
        for i in nums:
            if i not in frequencyForNum:
                frequencyForNum[i] = 1
            else:
                frequencyForNum[i] += 1
        for key, val in frequencyForNum.items():
            if val not in frequencyList:
                frequencyList[val] = [key]
            else:
                frequencyList[val].append(key)
        
        result = []
        for i in range(len(nums), 0, -1):
            if i in frequencyList:
                result.extend(frequencyList[i])
            if len(result) >= k:
                break
        return result
```
