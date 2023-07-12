# Majority Element

## Understanding the problem
Write a function that takes in a non-empty, unordered ```array``` of positive integers and returns the array's
majority element ```without sorting the array and without using more than constant space.```

An array's majority element is an element of the array that appears in over half of its indices. Note that the
most common element of an array (the element that appears the most times in the array) isn't necessarily
the array's majority element; for example, the arrays ```[3, 2, 2, 1]``` and ```[3, 4, 2, 2, 1]``` both have
```2``` as their most common element, yet neither of these arrays have a majority element, because neither
```2``` nor any other element appears in over half of the respective arrays’ indices.

You can assume that the input array will always have a majority element.

```
Sample Input:
array = [1,2,3,2,2,1,2]

Sample Output:
2  // 2 appears in 4/7 array indices making it the majority element
```

## Naive Approach
* <ins>Using O(n) space</ins> -> use dictionaries
* <ins>Using sorting</ins> -> return the middle elemeent
* Both these approaches are not allowed as per the question.

## Optimized Approach - 1
* <ins>Without sorting the array and using more than constant space:</ins>
* Break down this problem into sub-problems. Look for the majority element in sub-arrays instead of the entire array. How do we do that?
  * Consider the 1st element in a sub-array as the ```majority``` element and count of majority element as ```1```.
  * Compare ```majority``` with every other element in the array until ```count == 0```.
  * Then we increment the ```count``` for the copy of this element we come across in the array, and decrement the ```count``` for every other element.
  * If the ```count``` is 0, this means that in the sub-array from the majority element to the current element, there was no majority.
  * Assign the ```majority``` element to the 1st element in the next sub-array and continue the process.
  * Once the loop is over, If the ```count``` is greater than or equal to 1, this means that a majority element is found.
  * <ins>This works cause after all increments and decrements, the majority element constitutes for more than half of the array, so the count would always be greater than or equal to 1.</ins>
* Keep in mind:
  * Since the majority element is present in more than half of the array, it can end up as the last element or 2 or more of it's occurences can appear next to each other.
  * If it ends up as the sole last element without any consecutive occurence to the left of it, the sole element itself forms a subarray and thus it is a majority in that.

### Python Implementation:
```python
def majorityElement(array):

    # to keep track of the count of the majority element in a sub-array
    count = 1
    # to keep track of the majority element
    majority = array[0]
    
    if len(array) == 1:
        return majority
    
    for i in range(1, len(array)):  # O(n)
        # decrement for other elements
        if array[i] != majority:
            count -= 1
        # increment for copy elements
        else:
            count += 1
        # change sub-array if count is 0
        if count == 0:
            majority = array[i+1]

    # If the count is greater than or equal to one, this means that you have found your majority element
    if count >= 1:
        return majority
```

### Example-based Understanding:
Sample Input: array = [1,2,3,2,2,1,2]
```
STEP-1:
-> Initially, majority = 1, count = 1
-> Compare majority to every other element until count = 0
-> 2 != 1, thus, majority = 1, count = 1-1 = 0
-> Since 0 is reached, there is no majority in the sub-array of [1,2]. Start with the next sub-array.

STEP-2:
-> Majority = 3, count = 0
-> Compare 3 with 3, count = 0+1 = 1
-> Compare 3 with 2, count = 1-1 = 0
-> No majority in sub-array [3,2]

STEP-4:
-> Majority = 2, count = 0
-> 2 == 2, count = 0+1 = 1
-> 2 != 1, count = 1-1 = 0
-> No majority in sub-array [2,1]

STEP-5:
-> Majority = 2, count = 0
-> 2 == 2, count = 0+1 = 1
-> Majority in sub-array [2] is 2.
-> Return 2
```
Similar steps can be followed for all kinds of input arrays (Eg: [1,1,1,2,2,2,2], where sub-arrays are [1,1,1,2,2,2] and [2]).

### Complexity Analysis:
1. Time Complexity = ```O(n)``` as we loop through the input array of size 'n' only once.
2. Space Complexity = ```O(1)``` as we avoided the use of any additional data structures.

## Optimized Approach - 2
This approach uses <ins>bitwise operators</ins>. **The logic here is that we are basically checking each bit at all the 32 indices of all the array elements to find the bits which constitute the majority at each bit index.**

<ins>Keep in mind</ins>: The value of bit at index n (```currentBit```) is 2^n (```currentBitValue```) if there is a ```setBit``` at that index.

This is better explained with the help of an example: Sample Input = [1,2,3,2,2,1,2]

### Explanation
Start iterating from 0 to 32 as an integer is usually 32 bits long.

**VARIABLES:**
*  ```currentBit``` : Index of each bit of an array element starting.
*  ```currentBitValue``` : To check whether value at index of the desired element is a set bit (1 is present or not at that position).
*  ```count``` : To keep track of the number of elements which have a set bit in the same position as currentBitValue.
*  ```setBit``` : To check whether bit is set at index = currentBit for a particular array element.
*  ```num``` : particular array element
*  ```majority``` : to keep track of the bit values of the majority element

**STEPS:**
1. ```currentBit = n```
2. ```currentBitValue = 1 << currentBit``` (basically means, 2^currentBit) = ...0001 << 0 = ...0001 : meaning, bit is set at index 0
3. Check for setBit in array elements: ```setBit = currentBitValue & num```
4. If bit is set at index n, then increment the ```count```. If not, continue to the next element.
5. Check for setBit for all array elements (num) at index n and increment count accordingly.
6. If the count is greater than half the length of the array, this means that the majority array element has 1 (setBit) at index n.
7. So, we add the ```currentBitValue``` (2^n) at that index to ```majority = majority + currentBitValue.```
8. The above step basically means that we are finding the value of the majority element by adding it's constituent bit values (2^n) where the bit is set (n) (similar to ```binary to decimal conversion```)

**EXAMPLE:**
```
-> currentBit = 0 = index
-> currentBitValue = (..001) << 0 = (..001) or 2^0, meaning that the bit is set at index 0.
-> num = 1, setBit = (..001) & (..001) = (..001). Bit is set at index 0, count = 0+1 = 1.
-> num = 2, setBit = (..001) & (..010) = (..000). Bit is not set at index 0, continue.
-> Continue for all array elements to get count = 3.
-> But 3 < 7/2, thus we move onto the next index and set count = 0 again.

-> currentBit = 1 = index
-> currentBitValue = (..001) << 1 = (..010) or 2^1, meaning that the bit is set at index 1.
-> num = 1, setBit = (..010) & (..001) = (..000). Bit is not set at index 1, continue
-> num = 2, setBit = (..010) & (..010) = (..010). Bit is set at index 1, count = 0+1 = 1
-> Continue for all array elements to get count = 5 (since num = 3 also has a setBit at index 1).
-> Since 5 > 7/2, majority = 0 + currentBitValue = 0 + 2 = 0 + 2^1 = 2 (similar to binary to decimal conversion)
```

### Python Implementation:
```python
def majorityElement(array):

    majority = 0
    
    # currentBit: index of each bit of an array element starting
    for currentBit in range(32):  # O(32)
        
        # Value for each index position like 2^(currentBit)
        # To check whether value at index currentBit of the desired element is a set bit (1 is present or not at that position)
        currentBitValue = 1 << currentBit
        # To keep track of the number of elements which have a set bit in the same position as currentBitValue
        count = 0

        # loop through array elements to check for set bit at index currentBit
        for num in array:  # O(n)
            # to find set bit
            setBit = currentBitValue & num
            # if bit is set
            if setBit != 0:
                count += 1

        # if bit is set at index currentBit for more than half the elements, then add the currentBitValue to the answer
        if count > len(array) / 2:
            majority += currentBitValue

    return majority
```

### Complexity Analysis:
1. Time Complexity = O(32*n) ~ ```O(n)``` as given in the code.
2. Space Complexity = ```O(1)``` as there are no additional data structures used.

## Key Takeaways:
1. Divide a complex problem into smaller sets of problems and analyse it. Here, we checked the presence of a majority element in small sub-arrays.
2. Try to think of using bitwise operations as a viable approach even though it might seem daunting.
3. You can check whether there is a copy of an element using the ``setBit``` concept.
