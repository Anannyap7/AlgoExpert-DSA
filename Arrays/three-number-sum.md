# Three Number Sum

## Understanding the problem
Write a function that takes in a non-empty array of distinct integers and an integer representing a target sum. The function should find all triplets in the array that sum up to the target sum and return a two-dimensional array of all these triplets. The numbers in each triplet should be ordered in ascending order, and the triplets themselves should be ordered in ascending order with respect to the numbers they hold.

If no three numbers sum up to the target sum, the function should return an empty array.

## Approach-1
This follows a similar mathematical intuition ```(thirdNum = sum - (firstNum + secondNum))``` used for the two number sum problem. Basically, we can store these values as key:value pairs in a hashtable/dictionary (```secondNum : thirdNum```) and check whether the third number is present as a key in this hashtable. This verification is done in order to ensure that the third number is an element in the given array. Apart from this, it is also important to check whether the key is equal to the value, as we want to ensure that the number is not repeated while forming a triplet (no duplication).

### Algorithm:
1. Create an empty list: ```triplets```.
2. Initialize 2 iterators to account for the first value and the second value in the list.
3. Inside the first iterator, assign the first value as ```array[i] = firstNum``` and create an empty hashtable/dictionary ```hashtable = {}``` which resets according to the position of the first value.
4. Create another iterator inside the initial one to account for the second value of the triplet ```array[j] = secondNum```. This iterator starts from the value after the first number to avoid repetition of triplets (since, ```[-8,2,6] = [2,-8,6]```).
5. Estimate the third value using the mathematical intuition explained above: ```(thirdNum = targetSum - (firstNum + secondNum))``` and check whether the value corresponding to third number as a key is present in the dictionary or not as well as ensure that the key-value pairs are not equal to each other (second number should not be equal to the third number to avoid repetition).
6. Once the conditions are satisfied, append the sorted list of values in ```triplets```.
7. Return the completely sorted triplet.

### Python Implementation:
```python
def threeNumberSum(array, targetSum):

    triplets = []
    
    # O(n^2)
    for i in range(len(array)):
        firstNum = array[i]
        hashtable = {}
        for j in range(i+1, len(array)):  # ensures no duplication
            secondNum = array[j]
            thirdNum = targetSum - (firstNum + secondNum)
            hashtable[secondNum] = thirdNum
            check = hashtable.get(thirdNum)
            if check != None and secondNum != thirdNum:
                # takes constant time complexity since the sort is limited to fixed 3 numbers
                triplets.append(sorted([firstNum, secondNum, thirdNum]))
    
    triplets.sort()  # O(nlogn)
    
    return triplets
```

### Complexity Analysis:
1. Time Complexity: O(n^2) + O(nlogn) -> nested for-loop + sorting ~ ```O(n^2)```.
2. Space Complexity: ```O(n)``` -> triplets array

## Approach-2
Whenever a problem gets too complicated, **sort** the array and use **pointers** to see whether that can be an easier approach. The concept used in this case is ```comparison```. Using the current, left and right pointers which point the first, second and third element of the triplet; compare its sum with the given targetSum. Based on this, move the left and right pointers accordingly to bring the current sum equal to the target sum.

### Algorithm:
1. Initialize an empty list ```triplets```.
2. Sort the main array.
3. Iterate through the elements of the array and create ```left``` and ```right``` pointers. The left pointer points to current element + 1, while the right pointer points to the end of the array.
4. While the left pointer < right pointer, find the ```currentSum``` of the elements and compare it to the ```targetSum```.
5. If current sum is less that the target sum, then move the left pointer forward in the increasing direction to increase the current sum. In case of vice versa, move the right pointer backward in the ddecreasing direction to decrease the current sum.
6. Add the elements pointing to the iterator and left and right pointers to the triplets array in case ```currentSum == targetSum```.
7. Return the final set of triplets once main loop ends.

### Python Implementation:
```python
def threeNumberSum(array, targetSum):

    triplets = []
    array.sort()
    
    for i in range(len(array)-2):  # O(n^2)
        left = i+1
        right = len(array) - 1
        while left < right:
            currentSum = array[i] + array[left] + array[right]
            # move the left pointer towards the increasing value
            if currentSum < targetSum:
                left += 1
            # move the right pointer towards the decreasing value
            elif currentSum > targetSum:
                right -= 1
            # since we have achieved the targetSum, move both the pointers simultaneously to maintain the order
            else:
                triplets.append([array[i], array[left], array[right]])
                left += 1
                right -= 1
    
    return triplets
```
### Complexity Analysis:
1. Time Complexity: ```O(n^2)``` -> for each main iteration, left and right pointers loop through 'n' elements.
2. Space Complexity: ```O(n)``` -> triplets array

## Key Takeaway:
When faced with a ```n-sum``` problem, try to think in terms of pointers apart from the usual mathematical approach.
