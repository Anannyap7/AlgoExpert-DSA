# Smallest Difference

## Understanding the problem
Write a function that takes in two non-empty arrays of integers, finds the pair of numbers (one from each
array) whose absolute difference is closest to zero, and returns an array containing these two numbers,
with the number from the first array in the first position.

Note that the absolute difference of two integers is the distance between them on the real number line. For
example, the absolute difference of -5 and 5 is 10, and the absolute difference of -5 and -4 is 1.

You can assume that there will only be one pair of numbers with the smallest difference.

## Optimized Approach
As discussed earlier, it is always better to sort the given arrays and use pointers in case of ```sum``` or ```difference``` related problems.
Using pointers in a sorted array, you can apply the ```comparison``` method to increment or decrement pointer positions and keep track of the current difference.
The smallest difference would be the least value of this current difference.

### Algorithm:
1. Sort both the given arrays.
2. Initialize 2 pointers at the start of both arrays: ```ptr1``` and ```ptr2```.
3. Create a variable which keeps track of the smallest difference ```smallestDiff``` and an empty list ```smallestPair``` which returns the numbers with the least difference. Assign infinity (highest value) to the variable which can be changed on comparison.
3. Loop through both the arrays until one of them comes to an end. Carry out steps 4-7 inside the loop.
4. Calculate the absolute ```currentDiff``` between elements which correspond to ptr1 and ptr2 (```num1``` and ```num2```).
5. Compare the current different with the smallest difference and assign the least among the two to ```smallestDiff```. Simulatenously, append the 2 elements corresponding to the least difference to ```smallestPair```
6. In case ```num1``` is less than ```num2```, move ptr1 in the forward direction towards the increasing element to decrease the current difference. For array2 element less than array1 element, move ptr2 towards the increasing direction to decrease the current difference.
7. Return the elements which have the same value as the elements with the smallest difference.
8. Otherwise, return ```smallestPair``` after the loop ends.

### Python Implementation:
```python
def smallestDifference(arrayOne, arrayTwo):
    
    arrayOne.sort() # O(nlogn)
    arrayTwo.sort() # O(mlogm)
    ptr1 = 0
    ptr2 = 0
    smallestDiff = float("inf")
    smallestPair = []

    while ptr1 < len(arrayOne) and ptr2 < len(arrayTwo): # O(n) and O(m)
        num1 = arrayOne[ptr1]
        num2 = arrayTwo[ptr2]
        currentDiff = abs(num1 - num2)
        # if num1 < num2, since the array is sorted we move on to the next element
        # which would be larger than the current num1 and hence result in a smaller difference
        if num1 < num2:
            ptr1 += 1
        # if num1 > num2, we move on to the next element in arrayTwo which would be
        # larger than the current num2 in order to decrease the value of difference
        elif num1 > num2:
            ptr2 += 1
        else:
            return [num1, num2]
            
        if smallestDiff > currentDiff:
            smallestDiff = currentDiff
            smallestPair = [num1, num2]

    return smallestPair
```

### Complexity Analysis:
1. Time Complexity: (O(nlogn) + O(mlogm)) + (O(n) + O(m)) -> sorting + looping ~ ```O(nlogn) + O(mlogm)```, where n and m are the number of elements in array1 and array2
2. Space Complexity: ```O(1)``` since array of fixed length (2) is returned.

## Key Takeaway:
Sorting the array and using pointers always comes in handy in case of ```sum``` or ```difference``` type of questions.
