# Longest Peak

## Understanding the problem
Write a function that takes in an array of integers and returns the length of the longest peak in the array. 
A peak is defined as adjacent integers in the array that are **strictly increasing** until they reach a tip (the highest value in the peak), at which point they become **strictly decreasing**. 
At least three integers are required to form a peak. For example, the integers ```[1, 4, 10, 2]``` form a peak, but the integers ```[4, 0, 16]``` don't and neither do the integers [1, 2, 2, 0]. 
Similarly, the integers ```[1, 2, 3]``` don't form a peak because there aren't any strictly decreasing integers after the 3.

```
Sample Input
array = [1, 2, 3, 3, 4, 8, 10, 6, 5, -1, -3, 2, 3]
Sample Output.
6 // 0, 10, 6, 5, -1, -3
```

## Optimized Approach
You could think of solving the problem using booleans like isIncreasing and isDecreasing, like in the monotonic array problem. However, this would complicate the
problem by having multiple conditions and make it difficult to understand the code.

To eliminate this issue, the best approach is to always use ```left and right pointers```. Since finding the ```tipping point``` can help find the peak of the array,
try to think of using the left and right pointers around that tip.

**LOGIC:**
* Values to the left of the tip must be decreasing is backwards (<--) direction.
* Value to the right of the tip must be decreasing in forwards (-->) direction.
* After completing a peak, jump to the element next of the peak.
* Compare different peak lengths to find the maximum one.

### Algorithm:
1. Define the ```longestPeak``` to be 0 initially.
2. Initialize an iterator ```i=1```, since we need to compare the current element with the previous and next element to search for the tip.
3. Loop through until i becomes greater than or equal to the length of the array.
4. Inside the loop, compare the ```ith``` or current element with the previous ```(i-1)th``` and the next ```(i+1)th``` element.
5. In case the current element is greater than the previous and next element, then assign ```isPeak``` a boolean ```True``` which means that the tip has been found. If not, conitnue looping until you find one.
6. Once the peak is found, then compare the previous element with the element before it (```left = i-2```) and the next element with the element next to it (```right = i+2```).
7. If ```array[left] < array[left+1]```, the elements are decreasing in <-- direction. Decrement the left pointer to continue the comparison in backwards direction.
8. If ```array[right] < array[right-1]```, the elements are decreasing in --> direction. Increment the right pointer to continue the comparison in forwards direction.
9. The comparison in steps 7 & 8 stops when the peak is complete.
10. Calculate the length of the ```currentPeak``` by using the left and right pointers.
11. Compare this current peak with the longest peak and assign the maximum among them to ```longestPeak```.
12. Assign the iterator i to the right pointer and repeat steps 5-11 for elements next to the current peak, until the loop ends.
13. Return the resulting peak.

### Python Implementation:
```python
def longestPeak(array):

    longestPeak = 0
    i = 1

    while i < len(array) - 1:  # O(n)
        
        # check for peak/tipping point
        isPeak = array[i-1] < array[i] and array[i] > array[i+1]
        if not isPeak:
            i += 1

        else:
            # check for decreasing elements in backwards (<--) direction
            left = i-2
            while left >= 0 and array[left] < array[left+1]:
                left -= 1
            
            # check for decreasing elements in forwards (-->) direction
            right = i+2
            while right < len(array) and array[right] < array[right-1]:
                right += 1

            # find the current peak
            currentPeak = right - left - 1

            # compare current peak with longest peak
            longestPeak = max(currentPeak, longestPeak)
                
            # start the next sequence - explore elements to the right of the peak
            i = right

    return longestPeak
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` as we loop once through array of size 'n'.
2. Space Complexity: ```O(1)``` since we are only returning a value.

## Key Takeaway:
1. Whenever a TRAVERSAL problem gets too complicated always think in terms of 2 pointers (left and right in most cases).
2. Here, instead of having to go through so many conditionals, the better way is to use a 2nd pointer going in either direction of the peak point till the peak condition provided in the question keeps getting fulfilled.
