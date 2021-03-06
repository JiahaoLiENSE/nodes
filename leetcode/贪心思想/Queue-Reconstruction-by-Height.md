### 406. Queue Reconstruction by Height
#### Description
`You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.`  
`Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).`  
#### Example
Input: `people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]`  
Output: `[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]`  
Explanation:  
```
Person 0 has height 5 with no other people taller or the same height in front.
Person 1 has height 7 with no other people taller or the same height in front.
Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
Person 3 has height 6 with one person taller or the same height in front, which is person 1.
Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
Person 5 has height 7 with one person taller or the same height in front, which is person 1.
Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.
```
#### Solving Logic
```
1. Between different h, pick person with larger h first (H in DESC order): [7,0], [6,2], [5,1]
2. Between different k, pick person with smaller k first (in the same h) (K in ASC order): [7,0], [5,1], [6,2]
3. we put the person with larger k and then put the person with smaller k, the situation of the former will be changed by the latter one.
```
#### Java
```java
/*
    Java list.add(int index, element) -> https://www.journaldev.com/33297/java-list-add-addall-methods
*/
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        if(people.length == 1) return people;
        
        int[][] result = new int[people.length][];
        Arrays.sort(people, (a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);    // after: [[7,0], [7,1], [6,1], [5,0], [5,2], [4,4]]
        ArrayList<int[]> queue = new ArrayList<>();
        // arr[1] is reading int[] (here is [7,0]) index of 1 which is index[1] = 0. Then insert [7,0] into array at index 0
        for(int[] arr : people) {
            queue.add(arr[1], arr);                     // [[7,0]] (insert [7,0] at index 0);
        }                                               // [[7,0], [7,1]] (insert [7,1] at index 1);    
                                                        // [[7,0], [6,1], [7,1]] (insert [6,1] at index 1);
        for(int i = 0; i < people.length; i++) {        // [[5,0], [7,0], [6,1], [7,1]] (insert [5,0] at index 0);
            result[i] = queue.get(i);                   // [[5,0], [7,0], [5,2], [6,1], [7,1]] (insert [5,2] at index 2);
        }                                               // [[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]] (insert [4,4] at index 4).
        return result;
    }
}
```
#### Python
```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        if len(people) == 1 or not people:
            return people
        
        people.sort(key=lambda x: (-x[0], x[1]))
        result = []
        for arr in people:
            result.insert(arr[1], arr)
        return result
```
