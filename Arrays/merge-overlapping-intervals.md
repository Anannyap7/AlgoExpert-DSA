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
* Sample Input:
intervals = [[1,2],[3,5],[4,7],[6,9],[9,10]]
Sample Output:
[[1,2],[3,8],[9,10]]

* Sample Input:
intervals = [[100,105],[1,104]]
Sample Output:
[[1,105]]

* Sample Input:
intervals = [[1,22],[-20,30]]
Sample Output:
[[-20,30]]
```

## Appproach
Pretty Straightforward. Simply, check the last element of the current sub-array with the first element of the next sub-array. In case the former is larger
than the latter, then merge these 2 arrays.

### Algorithm:
1. 
