# Zero Sum Subarray

## Understanding the problem
You're given a list of integers nums . Write a function that returns a boolean representing whether there
exists a zero-sum subarray of nums.

A zero-sum subarray is any subarray where all of the values add up to zero. A subarray is any contiguous section of the array. 
For the purposes of this problem, a subarray can be as small as one element and as
long as the original array.

```
Sample Input:
nums = [-5, -5, 2, 3, -2]

Sample Output:
True  // the sub-array [-5,2,3] sums up to 0
```

## Approach
Since we need to find the subarray which adds up to 0, we cannot sort the array. To keep track of the current sum, we can use a ```set```. 
There are 3 ways to get 0 sum:
* Continuous array elements which sum up to 0.
* 0 is already present in the input array.
* Discountinuous array elements (forming a subarray) which sum up to 0.
The first 2 conditions are easy enough to think about. For the third condition, check if the sum from the starting element to some other element (x) give the same
sum as the sum from the starting element to another element (y):
```
If sum(a[0] to a[x]] = value and sum(a[0] to a[y]) = same value,
then sum(a[0] to a[x]) - sum(a[0] to a[y]) = 0
meaning, sum(a[0] to a[x]) + = sum(a[0] to a[y])
```
**[So, if the current sum found is already present in the set, this means that there is a 0 sum in the array.]**

### Algorithm:
1. Initialize a ```sumSet``` which contains 1 element initially (```0```). This is done just in case ```nums``` contains 0 or consecutive numbers add up to 0.
2. Create a variable to keep track of the current sum: ```currentSum```.
3. Iterate through the array elements.
4. Find the sum of elements from 0 to i (currentSum)
5. Check whether ```currentSum``` is present in the set. If yes, then return ```True``` as this means that a 0 sum exists in the input array.
6. If not, add the ```currentSum``` to the ```sumSet```.
7. Once the loop ends and there are no 0 sums, return ```False```.

### Python Implementation:
```python
def zeroSumSubarray(nums):

    sumSet = {0}   
    currentSum = 0
    for num in nums:  # O(n)
        currentSum += num
        if currentSum in sumSet:
            return True
        else:
            sumSet.add(currentSum)
            
    return False
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` since we loop through array of size 'n' only once.
2. Space Complexity: ```O(n)``` since we use a set with max possible size of 'n'.

## Key Takeaway
Think of a mathematical approach during ```sum``` type questions, in case the concept of ```pointers``` doesn't help with the solution.
