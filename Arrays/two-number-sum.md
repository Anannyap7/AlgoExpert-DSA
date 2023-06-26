# TWO NUMBER SUM

## Understanding the problem
Given an array of distinct integers and an integer representing the target sum, find 2 distinct integers from the array which equate to the target sum and return them in an array. If no two numbers sum up to the target sum, then return an empty array.

## Approach-1: Brute Force

This approach traverses an array of size 'n' n times. That is, for each element in the array, we are iterating through the rest of the array in order to find the required target sum.
Since we are only initializing a result array of fized size 2, therefore space complexity remains O(1).

### Python Implementation:
```python
def twoNumberSum(array, targetSum):
  result = []
  for i in range(len(array)):  # O(n)
    for j in range(i+1, len(array)):  # O(n)
      if array[i] + array[j] == targetSum:
        result.append(array[i])
        result.append(array[j])
  return result
  ```

### Complexity Analysis: 
1. Time Complexity: ```O(n^2)``` where 'n' is the size of the array
2. Space Complexity: ```O(1)```

## Approach-2: Optimized approach using hashtables

Instead of using brute-force, it is always better to think of ```mathematics-based``` alternatives. In this case, think of approaching the problem logically: how would you find y given x+y = sum?
To put it simply, ```y = sum - x```. We have established our logic. Now our next aim is to reduce the time complexity from O(n^2) -> O(n), which can be done using hashtables. In the brute-force approach, the repeated iteration on the same array for each numbers complement takes O(n^2) time; however, storing key-value pairs ```(number : complement)``` in a hashtable ensures that we loop through the array only once (O(n)). Additionally, searching in hashtable takes constant time as opposed to searching in a list. So, the final time complexity is **O(n) * O(1)**.

### Algorithm:
1. Create an empty hashtable and store key-value pairs as ```x : targetSum - x```.
2. Check if the complement (= targetSum - x) of each number is present as a key in our hashtable. This would mean that the complement is a part of the main array and not just any random number.
3. There is one last condition to be met: 2 numbers should be distinct. Add a condition to ensure that the complement is not the current number.

### Python Implementation:
```python
def twoNumberSum(array, targetSum):
  hashtable = {}
  for x in array:  # O(n)
    hastable[x] = targetSum - x
    y = hashtable.get(targetSum - x)  # O(1)
    if y != None and targetSum - x != x:  # O(1)
      return [x, hashtable[x]]
  return []
```
### Complexity Analysis:
1. Time Complexity: O(n) * O(1) = ```O(n)``` where 'n' is the size of the array.
2. Space Complexity: ```O(n)``` due to hashtable initialization.

## Key Takeaways
1. Use a mathematical-based logic instead of brute-force in 'number sum' sort of questions.
2. Searching in a dictionary/hashtable takes up constant time complexity as opposed to searching in a list/array.
