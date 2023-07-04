# First Duplicate Number

## Understanding the problem
Given an array of integers between ```1``` and ```n```, inclusive, where n is the length of the array, write a function that returns the first integer that appears more than once (when the array is read from left to right). 
In other words, out of all the integers that might occur more than once in the input array, your function should return the one whose first duplicate value has the minimum index. 
If no integer appears more than once, your function should return ```-1```. Note that you're allowed to mutate the input array.

## Approach-1 (O(N) and O(n) Time and Space Complexity)
We use a ```set``` to track array elements since we only need to return the first occuring repeated element. So, if the current element is already present in the set, return that element and end the code.

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
There is always a reason why extra information is given in questions. In this case, it has been explicitly mentioned that:
* The array elements are between ```1``` to ```n```, where 'n' is the length of the array.
* We are allowed to mutate the input array.
So, let's take advantage of the given information to our benefit and reduce the space complexity from O(n) to O(1). The logic behind this approach is that we can access different array elements using the value of the current element: ```value-1```. 1 is subtracted since the array elements range from ```1 to n``` while the indices of the array range from ```0 to n-1```.
Simply make the element at position ```value-1``` negative. This is a way of flagging that index and making note that we have encountered the element corresponding to that index position previously. 

### Algorithm:
1. Iterate through the array elements.
2. Inside the loop, assign ```index``` as the absolute value of current value of the element - 1 (```abs(array[i]-1)```). This is done so that if an element is made negative, it would not affect the indexing.
3. Access the value at the index of the current element and make it negative in case the element at that position is positive.
4. If the value at that index is already negative, this means that we have already encountered the current element and it is a duplicate.
5. Return the absolute value of that duplicate (as it may have been made negative when some other element accessed its index).

### Python Implementation:
```python
def firstDuplicateValue(array):

    # To use the given information, subtract all array elements by 1 since the indices
    # are from 0 to n-1, while the array elements are from 1 to n.
    for i in range(len(array)):  # O(n)
        index = abs(array[i]) - 1
        # check whether element at index is negative or not
        if array[index] > 0:
            array[index] = -array[index]
        else:
            return abs(array[i])
    return -1
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` since we loop once through an array of size 'n'.
2. Space Complexity: ```O(1)``` since no data structure was created.

## Key Takeaway:
* Make use of every single detail given in the prompt. They will never give extra details unecessarily.
* If all numbers in an array are between 1 and n, you can use the values itself to reference index positions in the array.
