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

