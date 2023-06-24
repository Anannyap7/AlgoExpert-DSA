# VALIDATING SUBSEQUENCE

## Understanding the problem
Given 2 non-empty arrays of integers, write a function that determines whether the second array is a subsequence of the first array. A subsequence of an array is a set of numbers in the array which are not necessarily adjacent in the array but they are in the same order as they appear in the array.

## Optimized Approach

You can follow the brute-force approach of travering through each element of the 1st array and comparing it with each element of the 2nd array (nested for loops). However, this would result in O(n^2) of time complexity. Hence, we have to look for other alternatives to approach this problem. So the first thing that comes up in your mind is that 'How can I loop through only one of the arrays?'.

This question makes it clear that only looping through the main array would result in O(n) time complexity. What about the subsequence array? Since the purpose here is to determine whether the second array is a subsequence of the first array, all you need to do is compare elements from the 1st array to elements from the second array simultaneously. This is remarkably better than the brute-force approach of taking 1 element of the first array and comparing it with each element of the second array. 

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
1. Time Complexity: O(n^2) where 'n' is the size of the array
2. Space Complexity: O(1)

## Approach-2: Optimized approach using hashtables

Instead of using brute-force, it is always better to think of mathematics based alternatives. In this case, think of approaching the problem logically: how would you find y given x+y = sum?
To put it simply, y = sum - x. We have established our logic. Now our next aim is to reduce the time complexity from O(n^2) -> O(n), which can be done using hashtables. In the brute-force approach, the repeated search for each numbers complement takes O(n^2) time; however, searching in hashtables corresponds to a constant complexity as opposed to searching in lists (O(n)). So, the final time complexity is O(n) * O(1): Traversing through the array to create hashtable * searching in hashtable.

**Algorithm:**
1. Create an empty hashtable and store key-value pairs as x : targetSum - x.
2. Check if the complement (= targetSum - x) of each number is also present in our hashtable. 
3. There is one last condition to be met: 2 numbers should be distinct. In order to ensure that the complement is not the current number itself, we put another condition in the 'if' statement stating our requirements.

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
1. Time Complexity: O(n) where 'n' is the size of the array
2. Space Complexity: O(n)
