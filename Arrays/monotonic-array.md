# Monotonic Array

## Understanding the problem
Write a function that takes in an array of integers and returns a boolean representing whether the array is
monotonic.

An array is said to be monotonic if its elements, from left to right, are entirely non-increasing or entirely
non-decreasing.

Non-increasing elements aren't necessarily exclusively decreasing; they simply don't increase. Similarly,
non-decreasing elements aren't necessarily exclusively increasing; they simply don't decrease.

```Sample Input: [-1, -5, -10, -1100, -900, -1101, -1102, -9001]
Sample Output: False
Reason: Function started decreasing initially, but at one point increased from -1100 to -900 

Sample Input: [1, 1, 2, 3, 4, 5, 5, 5, 6, 7, 8, 7, 9, 10, 11]
Sample Output: False
Reason: Function started increasing initially, but at one point decreased from 8 to 7.
```

Note that empty arrays and arrays of one element are monotonic.

## Approach-1:
The simplest way of doing this is to initialize 2 variables which a count of the number of non-increasing and non-decreasing elements in the array.
Check if any of the 2 values correspond to the length of the array to ensure that all elements are either non-increasing or non-decreasing. 
Return ```True``` in case this condition is satisfied.

### Algorithm:
1. Initialize 2 variables ```flag1``` and ```flag2``` to keep a count of the non-increasing and non-decreasing elements respectively.
2. Iterate through all the elements of the array.
3. Inside the loop, check whether the current element is less than or greater than the next element.
In case ```array[i] > array[i+1]``` then increment ```flag1``` since the elements are non-increasing.
In case ```array[i] < array[i+1]``` then increment ```flag2``` since the elements are non-decreasing.
In case both are equal, then increment both the variables since the elements could be non-increasing or non-decreasing.
4. Check whether either of the variables correspond to the total count of elements inside the array. Return ```True``` if true and ```False``` if false.
5. In case of length of array is 0 or 1, then it is automatically monotonic. Return ```True```

### Python Implementation:
```python
def isMonotonic(array):
    
    flag1 = 0  # to keep track of non-increasing elements
    flag2 = 0  # to keep track of non-decreasing elements
    
    for i in range(len(array)-1):  # O(n)
        if array[i] > array[i+1]:
            flag1 += 1
        elif array[i] < array[i+1]:
            flag2 += 1
        else:
            flag1 += 1
            flag2 += 1
    
    if flag1 == len(array)-1 or flag2 == len(array)-1 or len(array) <= 1:
        return True
        
    return False
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` as we loop through the given array of size 'n' once.
2. Space Complexity: ```O(1)``` since no data structure was initialized.

## Approach-2
An alternate way to keep track of non-increasing and non-decreasing numbers is to think in terms of ```direction```. 
If the direction is upwards/increasing and 0 initially and it suddenly changes its direction (upwards/decreasing) as we iterate through the remaining elements; this means that that array is not monotonic.
Same is true for vice-versa.

### Algorithm:
1. Assign initial direction as the difference between the 1st element and the 0th element: ```direction = array[1] - array[0]```.
2. Loop through the given array and find the direction between the current and previous element.
3. If ```direction == 0``` initially, then conitnue looping until this condition breaks.
4. Once direction != 0, find the ```difference``` between the current and the previous element moving forward.
When the direction breaks, i.e., when the sign of the initial direction differs from the sign of the current difference.
In case it does, this means that the non-increasing or non-decreasing direction breaks and the array is not monotonic (```False```).

### Python Implementation
```python
def isMonotonic(array):

    if len(array) <= 2:
        return True

    # if direction > 0 : non-decreasing, if direction < 0: non-increasing
    direction = array[1] - array[0]

    for i in range(2, len(array)):  # O(n)
        if direction == 0: # if both numbers are the same, the update the value of direction
            direction = array[i] - array[i-1]
            continue # continue until direction != 0
        if breaksDirection(direction, array[i-1], array[i]):
            return False

    return True

# if the sign of difference and direction differ: return True since it breaks direction
def breaksDirection(direction, previous, current):
    difference = current - previous
    if direction > 0:
        return difference < 0 # True if diff < 0 and False if diff > 0 
    else:
        return difference > 0 # True if diff > 0 and False if diff < 0
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` as we loop through the given array of size 'n' once.
2. Space Complexity: ```O(1)``` since no data structure was initialized.

## Key Takeaway:
```Direction``` is a new way of thinking while approaching arrays and comparing arrays.
