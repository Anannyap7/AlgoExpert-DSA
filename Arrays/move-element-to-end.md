# Move Element to End

## Understanding the problem:
You're given an array of integers and an integer. Write a function that moves all instances of that integer in
the array to the end of the array and returns the array.

The function should perform this in place (i.e., it should mutate the input array) and doesn't need to
maintain the order of the other integers.

## Optimized Approach
Think of how you would take the desired element from the array and place it in the last position? Moving of elements in the array without creating a new array can be done using ```swapping```.
Simply swap the desired element with a regular element present towards the end of the array. Keep tracking of the element to move and the position to place it in using 2 pointers.

### Algorithm:
1. Initialize two pointers ```left``` and ```right``` at the start and end of the array.
2. While the left pointer index is less than the right pointer index, loop through the array to perform the desired operations.
3. Inside the loop, check whether the rightmost element is the element ```toMove```. If yes, then decrement the right pointer since this element is already in the apporpriate position.
4. If not, swap the leftmost element to move with the rightmost element and increment the left pointer.
5. In case the leftmost element is not the desired element, move the pointer forwards as we don't need to modify regular elements.
6. Once the loop ends, return the modified ```array```.

### Python Implementation:
```python
def moveElementToEnd(array, toMove):

    left = 0
    right = len(array)-1

    while left < right:  # O(n)
        if array[right] == toMove:
            right -= 1
        else:
            if array[left] == toMove:
                array[left], array[right] = array[right], array[left]  # swapping
                left += 1
            else:
                left += 1

    return array
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` where n is the length of the input array.
2. Space Complexity: ```O(1)``` since no data structure has been initialized.

## Key Takeaway
Use ```swapping``` and ```pointers``` in case you want to move elements of the array without creating a new array.
