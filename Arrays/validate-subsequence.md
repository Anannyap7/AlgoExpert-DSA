# VALIDATING SUBSEQUENCE

## Understanding the problem
Given 2 non-empty arrays of integers, write a function that determines whether the second array is a subsequence of the first array. A subsequence of an array is a set of numbers in the array which are not necessarily adjacent in the array but they are in the same order as they appear in the array.

## Optimized Approach

You can follow the **brute-force approach** of travering through each element of the 1st array and comparing it with each element of the 2nd array (nested for loops). However, this would result in **O(n^2)** of time complexity. Hence, we have to look for other alternatives to approach this problem. It is clear that to reduce the time complexity to **O(n)**, we need to loop through each element of both the arrays only once. Since the purpose here is to determine whether the second array is a subsequence of the first array, all you need to do is **compare elements from the 1st array to elements from the second array simultaneously**. This is remarkably better than the brute-force approach.

### Algorithm:
1. Initialize a variable to keep track of the number of coinciding elements: ```count = 0```
2. Iterate through the main array and compare elements from the main array with elements from the sub-array using ```count``` as an iterator: ```array[i] with sequence[count]```.
3. If the values are equal, increase the ```count```.
4. If not, then continue iterating through the elements of the main array until you find an element which is also present in the sub-array.
5. Return ```True``` if the value of ```count``` equals to the length of the array and ```False``` otherwise. ```True``` means that all the elements of the sub-array are present in the main-array sequentially.

### Python Implementation
```python
def isValidSubsequence(array, sequence):
  count = 0
  for i in range(len(array)):  # O(n)
    if array[i] == sequence[count]:  # O(n)
      count += 1
    if count == len(subsequence):  # to stop looping as count value > last index of sub-array (after looping through the sub-array)
      return True
  return False
  ```

### Complexity Analysis: 
1. Time Complexity: O(n) -> main array + O(n) -> sub-array = ```O(2n) ~ O(n)``` where 'n' is the size of the main array
2. Space Complexity: ```O(1)``` for counter variable
