# Merge Overlapping Intervals

## Understanding the problem
Write a function that takes in a non-empty array of arbitrary intervals, merges any overlapping intervals,
and returns the new intervals in no particular order.

Each interval ```interval``` is an array of two integers, with ```interval[0]``` as the start of the interval and
```interval[1]``` as the end of the interval.

Note that back-to-back intervals aren't considered to be overlapping. For example, ```[1, 5]``` and ```[6, 7]```
aren't overlapping; however, ```[1, 6]``` and ```[6, 7]``` areindeed overlapping.

Also note that the start of any particular interval will always be less than or equal to the end of that interval.

```
Sample Input:
intervals = [[1,2],[3,5],[4,7],[6,9],[9,10]]
Sample Output:
[[1,2],[3,8],[9,10]]

Sample Input:
intervals = [[100,105],[1,104]]
Sample Output:
[[1,105]]

Sample Input:
intervals = [[1,22],[-20,30]]
Sample Output:
[[-20,30]]
```

## Appproach
Pretty Straightforward. Simply, check the last element of the current sub-array with the first element of the next sub-array. In case the former is larger
than the latter, then merge these 2 arrays.

### Algorithm:
1. Sort the input array to ensure proper merging of overlapping intervals.
2. Create an empty ```result``` array.
3. To keep track of the current interval which has been merged, initialize a variable ```current``` as the 1st interval of the input array.
4. Iterate through all the intervals in the array.
5. Assign the interval next to the current interval as ```next```.
6. To check whether intervals should be merged, compare the last element of the ```current``` interval with the first element of ```next``` interval.
7. In case the former is greater than the latter, then merge these two arrays. The merged interval should contain the first element of ```current``` and the ```max``` of the last element of the current and next interval. This is done to ensure that if the range current interval is larger than the range of the next interval, then the current interval is the output instead of a merge of the two intervals.
8. In case the above condition is not satisfied, then simply ```append``` the current interval in the ```result``` array and increment this interval.
9. Once the loop ends, return the output array.

### Python Implementation:
```python
def mergeOverlappingIntervals(intervals):

    # sort the interval elements to ensure that the intervals merge properly
    intervals.sort()
    
    result = []
    n = len(intervals)-1
    current = intervals[0]
    
    # for array with only 1 interval
    if n == 0:
        return intervals
        
    for i in range(n):  # O(n)
        next = intervals[i+1]
        if current[1] >= next[0]:
            current = [current[0], max(current[1], next[1])]
        else:
            result.append(current)
            current = intervals[i+1]
    result.append(current)
    
    return result
```

### Complexity Analysis:
1. Time Complexity: O(nlogn) + O(n) -> sorting + looping ~ ```O(nlogn)``` where 'n' is the size of the input array.
2. Space Complexity: ```O(n)``` where 'n' is the size of the output array.

## Key Takeaway
* Using a variable to keep track of the current events in the array makes understanding and coding easier.
* In case of overlapping questions, it is better to sort the input array first
