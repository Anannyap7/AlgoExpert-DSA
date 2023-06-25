# Sorted Squared Array

## Understanding the problem

Write a function that takes in a non-empty array of integers that are sorted in ascending order and returns a new array of the same length with the squares of the original integers also sorted in ascending order.

## Optimized Approach

This is not a straight-forward problem where you can simply square all the elements in the sorted array and return it. The presence of negative integers compicates this brute-force approach.
The **brute-force approach** of adding the squared integers from the main array to the new array and sorting the array (timsort) would take **O(n) + O(nlogn)** time complexity. But, we can still reduce the time complexity and make this algorithm more efficient.
Since the given array is already sorted, we can use **left and right pointers** at the start and end of the array to solve this problem. Using pointers would ensure traversal through the array once which would result in **O(n)** time complexity.

### Algorithm:
1. Initialize left and right pointers/variables at the start and end of the main array and create an empty array: ```left = 0 and right = len(array)-1```
2. Initialize another variable to keep track of the position of the element to be inserted at the end of the result array: ```pos = len(array)-1```
3. Now compare the absolute value of the integer on the left with the absolute value of the integer on the right.
4. If the integer on the left is greater than the integer on the right, add the squared left value to where ```pos``` points to (in result array) and move the left pointer forward.
5. If the integer on the right is greater than the integer on the left, add the squared right value to where ```pos``` points to (in result array) and move the right pointer backward.
6. After one iteration is complete, move the pointer in the new array backward to accomodate the second largest element.
7. Continue steps 3-6 till the right pointer crosses the left pointer.
8. Return the resulting array.

```python
import math

def sortedSquaredArray(array):
  result = [0] * len(array)
  left = 0
  right = len(array)-1
  pos = len(array)-1

  while left <= right:  # O(n)
    if abs(array[left]) > abs(array[right]):
      result[pos] = math.pow(array[left],2)
      left += 1
    else:
      result[pos] = math.pow(array[right],2)
      right -= 1
    pos -= 1

  retrun result
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` where 'n' is the size of the given array.
2. Space Complexity: ```O(n)``` for creating and storing the resulting array.
