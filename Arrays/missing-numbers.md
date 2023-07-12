# Missing Numbers

## Understanding the problem

You're given an unordered list of unique integers nums in the range ```[1, n]``` , where n represents the
length of ```nums + 2``` . This means that two numbers in this range are missing from the list.

Write a function that takes in this list and returns a new list with the two missing numbers, sorted
numerically.

```
Sample Input:
nums = [1,4,3]

Sample Output:
[2,5]  // n is 5, meaning that the complete list should be [1,2,3,4,5]
```

## Approach-1: Naive Approach (O(n) time and space complexity)
The simplest way of thinking would be convert the list of numbers into  ```set``` and iterate from 1 to n to check for the missing number. In case you find the missing number,
add it to the output array. However, this approach results in O(n) space complexity due to the set of ```nums``` which could be potentially avoided.

### Python Implementation:
```python
# O(n) time and space complexity
def missingNumbers(nums):
    
    set(nums)  # O(n) time and space
    n = len(nums)+2
    result = []
    
    for x in range(1, n+1):  # O(n) time
        if x not in nums:
            result.append(x)
    
    return result
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` as we loop through input array of size 'n' once.
2. Space Complexity: ```O(n)``` due to storing of input array into set.

## Approach-2: Optimized Approach-1
To efficiently find a [single] missing number you can take the ```difference``` of the sum of the input array and the sum of the expected array (range(1,n)).
Now how do we map this logic to find 2 missing numbers instead of 1?
* <ins>FACT: If you take the average of the 2 missing numbers, 1 missing number would be greater than the average and 1 would be less.</ins>
  * ```average = (expectedSum - numSum)//2```
  * Divide the expected array based on the average found into ```two halves```. One of the numbers would be on the left (less than average) and one on the right (more than average)
* <ins>Since we know that there is one missing number on each side of the expected array, we can treat each of the halves (left and right) as its own problem of finding a missing number</ins>
  * Add numbers less than the average (to the left of the avg) and greater than the average (to the right of the avg) for the expected array: ```leftExpectedSum``` and ```rightExpectedSum```.
  * Repeat the same for the input array: ```leftNumSum``` and ```rightNumSum```.
  * Take the left and right ```difference``` of the expected and input array to find the 2 missing numbers: ```num1``` and ```num2```.

### Python Implementation:
```python
def missingNumbers(nums):

    # find the sum of nums array
    numSum = sum(nums)  # O(n)
    
    # find the sum of the expected array
    expectedSum = 0
    n = len(nums) + 2
    for x in range(1,n+1):  # O(n)
        expectedSum += x

    # find the average value to create a division
    average = (expectedSum - numSum)//2

    # calculate the sum of the expected array to the left and right of the average
    leftExpectedSum = sum(range(1, average + 1))  # O(n)
    rightExpectedSum = sum(range(average + 1, n+1))  # O(n)

    # calculate the sum of the input array to the left and right of the average
    leftNumSum = 0
    rightNumSum = 0
    for num in nums:  # O(n)
        # left
        if num <= average:
            leftNumSum += num
        # right
        else:
            rightNumSum += num

    # Subtract the expected left sum with the nums left sum to get the first missing number.
    # Subtract the expected right sum with the nums right sum to get the second missing number.
    num1 = leftExpectedSum - leftNumSum
    num2 = rightExpectedSum - rightNumSum

    # return these 2 numbers
    return [num1, num2]
```

### Complexity Analysis:
1. Time Complexity = ```O(n)``` as seen in the code.
2. Space Complexity = ```O(1)``` as we are only returning a fixed array of size 2 and not using any additional space of size 'n'.

## Approach-2: Optimized Approach-2
This approach uses bitwise operators and their logic. <ins>The idea is to XOR the input array and the expected range to eliminate same values and find the resulting XOR
of both the missing values as ```resultXOR```.</ins>
Now, how do we find the missing values from their combination XOR? <ins>The idea used here is that bit would be set in one of the missing numbers and not the other.</ins>
* Find the rightmost ```setBit``` <ins>(digit 1 in brinary representation is known as set bit)</ins> of ```resultXOR```. This gives us a setBit at a particular position.
  * This is done by: ```resultXOR``` AND ```2's complement of resultXOR```.
* Check whether this ```setBit``` is present in the correct position (<ins>rightmost position</ins>) in the input and expected array.
  * This is done by taking the AND of a number from the expected array/input array with the set bit.
  * If the set bit is present, take the XOR of the values from the input array and expected range to find ```missing number 1```.
  * If not, perform the same operation for input and expected values which do not have the set bit to find ```missing number 2```.
* Once the loop ends, return the ```sorted``` resulting list of missing numbers.

### Example-based understanding
```
Sample Input: nums = [1,4,3]

The range here is from 1 to len(nums) + 2: 1 to 5: [1,2,3,4,5]

All the calculations will be done on decimals itself due to integer type of input. However the explanation is more intuitive in terms of binary representation.

1. resultXOR = 1^2^3^4^5^1^4^3 = same elements get cancelled out (XOR rule) and we are left with 2^5 = 0010 ^ 0101 = 0111
2. setBit = 0111 & -(0111) = 0111 & 1001 = 0001 -> rightmost set bit = 1
3. Check whether set bit 1 is present at position 3 or rightmost position of any of the input and expected elements. If set bit is present, i & setBit != 0.
4. Elements with set bit at position 3: 1 (0001 & 0001 = 1111), 3 (0011 & 0001 = 1101) and 5(0101 & 0001 = 1011) -> the outputs have 1 at position 3.
4. Thus, 1^3^5^1^3 (expected and input elements) = 5 -> one of the missing values.
5. Elements with no set bit at position 3: 2 (0010 & 0001 = 1100) and 4 (0100 & 0001 = 1010) -> the outputs don't have 1 at position 3
6. Thus, 2^4^4 (expected and input elements) = 2 -> the other missing value.
```

### Python Implementation:
```python
# Video Explanation - 2
def missingNumbers(nums):

    # XOR of the 2 missing nums
    resultXOR = 0
    n = len(nums) + 2
    
    # input array XOR expected array. 
    # Starting the loop with 0 since initial resultXOR(0) can get cancelled
    for i in range(0, n+1):  # O(n)
        resultXOR ^= i
        # for input array
        if i < len(nums):
            resultXOR ^= nums[i]

    # output array
    result = [0,0]

    # to find the rightmost bit for resultXOR we take its 2's complement, then AND it with its original value
    setBit = resultXOR & -resultXOR

    # To check whether the expected range has this bit set or not
    for i in range(0, n+1):  # O(n)
        # if i does not have the setBit in correct position
        if i & setBit == 0:
            result[0] ^= i
        # if i has the setBit in correct position
        else:
            result[1] ^= i
        # for input array
        if i < len(nums):
            if nums[i] & setBit == 0:
                result[0] ^= nums[i]
            else:
                result[1] ^= nums[i]

    return sorted(result)  # small time complexity since array is of size 2
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` as seen in code.
2. Space Complexity = ```O(1)``` as we are only returning a fixed array of size 2 and not using any additional space of size 'n'.
