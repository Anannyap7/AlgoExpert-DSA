# Majority Element

## Understanding the problem
Write a function that takes in a non-empty, unordered ```array``` of positive integers and returns the array's
majority element ```without sorting the array and without using more than constant space.```

An array's majority element is an element of the array that appears in over half of its indices. Note that the
most common element of an array (the element that appears the most times in the array) isn't necessarily
the array's majority element; for example, the arrays ```[3, 2, 2, 1]``` and ```[3, 4, 2, 2, 1]``` both have
```2``` as their most common element, yet neither of these arrays have a majority element, because neither
```2``` nor any other element appears in over half of the respective arrays’ indices.

You can assume that the input array will always have a majority element.

```
Sample Input:
array = [1,2,3,2,2,1,2]

Sample Output:
2  // 2 appears in 4/7 array indices making it the majority element
```

## Naive Approach
* <ins>Using O(n) space</ins> -> use dictionaries
* <ins>Using sorting</ins> -> return the middle elemeent
* Both these approaches are not allowed as per the question.

## Optimized Approach - 1
* <ins>Without sorting the array and using more than constant space:</ins>
* Break down this problem into sub-problems. Look for the majority element in sub-arrays instead of the entire array. How do we do that?
  * Consider the 1st element in a sub-array as the ```majority``` element and count of majority element as ```1```.
  * Compare ```majority``` with every other element in the array until ```count == 0```.
  * Then we increment the ```count``` for the copy of this element we come across in the array, and decrement the ```count``` for every other element.
  * If the ```count``` is 0, this means that in the sub-array from the majority element to the current element, there was no majority.
  * Assign the ```majority``` element to the 1st element in the next sub-array and continue the process.
  * Once the loop is over, If the ```count``` is greater than or equal to 1, this means that a majority element is found.
  * This works cause after all increments and decrements, the majority element constitutes for more than half of the array, so the count would always be greater than or equal to 1.
* Keep in mind:
  * Since the majority element is present in more than half of the array, it can end up as the last element or 2 or more of it's occurences can appear next to each other.
  * If it ends up as the sole last element without any consecutive occurence to the left of it, the sole element itself forms a subarray and thus it is a majority in that.

### Python Implementation:
```python
def majorityElement(array):

    # to keep track of the count of the majority element in a sub-array
    count = 1
    # to keep track of the majority element
    majority = array[0]
    
    if len(array) == 1:
        return majority
    
    for i in range(1, len(array)):  # O(n)
        # decrement for other elements
        if array[i] != majority:
            count -= 1
        # increment for copy elements
        else:
            count += 1
        # change sub-array if count is 0
        if count == 0:
            majority = array[i+1]

    # If the count is greater than or equal to one, this means that you have found your majority element
    if count >= 1:
        return majority
```

### Example-based Understanding:
1. Sample Input: array = [1,2,3,2,2,1,2]
```
STEP-1:
-> Initially, majority = 1, count = 1
-> Compare majority to every other element until count = 0
-> 2 != 1, thus, majority = 1, count = 1-1 = 0
-> Since 0 is reached, there is no majority in the sub-array of [1,2]. Start with the next sub-array.

STEP-2:
-> Majority = 3, count = 0
-> Compare 3 with 3, count = 0+1 = 1
-> Compare 3 with 2, count = 1-1 = 0
-> No majority in sub-array [3,2]

STEP-4:
-> Majority = 2, count = 0
-> 2 == 2, count = 0+1 = 1
-> 2 != 1, count = 1-1 = 0
-> No majority in sub-array [2,1]

STEP-5:
-> Majority = 2, count = 0
-> 2 == 2, count = 0+1 = 1
-> Majority in sub-array [2] is 2.
-> Return 2
```
Similar steps can be followed for all kinds of input arrays (Eg: [1,1,1,2,2,2,2], where sub-arrays are [1,1,1,2,2,2] and [2]).

### Complexity Analysis:
1. Time Complexity = ```O(n)``` as we loop through the input array of size 'n' only once.
2. Space Complexity = ```O(1)``` as we avoided the use of any additional data structures.

## Optimized Approach - 2
