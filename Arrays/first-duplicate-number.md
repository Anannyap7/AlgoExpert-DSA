# First Duplicate Number

## Understanding the problem
Given an array of integers between ```1``` and ```n```, inclusive, where n is the length of the array, write a function that returns the first integer that appears more than once (when the array is read from left to right). 
In other words, out of all the integers that might occur more than once in the input array, your function should return the one whose first duplicate value has the minimum index. 
If no integer appears more than once, your function should return ```-1```. Note that you're allowed to mutate the input array.

## Approach-1 (O(N) and O(n) Time and Space Complexity)
We use a ```set``` to track array elements since we only need to return the first occuring repeated element. So, if the current element is already present in the set
then return that element and end the code.

### Algorithm:
1. Create an empty set: ```check```.
2. Iterate through array elements.
3. Inside the loop, check whether the current integer in the array is already present in the set. If not, add that number in the set. If yes, return that number as the output.
4. Return -1 if there are no duplicate elements.

### Python Implementation:
```python
# O (N) Time and Space Complexity
def firstDuplicateValue(array):

    check = set()
    # if the current element you are looping through is already present in the set
    # return that number as the frist duplicate value
    for a in array:  # O(n)
        if a not in check:  # O(1)
            check.add(a)
        else:
            return a
    # if no integers appear more than once
    return -1
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` since we loop once through an array of size 'n'.
2. Space Complexity: ```O(n)``` since we store non-repeated array elements in a set.

## Approach-2 (O(n) and O(1) Time and Space Complexity)
