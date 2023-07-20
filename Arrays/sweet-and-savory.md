# Sweet and Savory

## Understanding the problem
You're hosting an event at a food festival and want to showcase the best possible pairing of two dishes
from the festival that complement each other's flavor profile.

Each dish has a flavor profile represented by an integer. A negative integer means a dish is sweet, while a
positive integer means a dish is savory. The absolute value of that integer represents the intensity of that
flavor. For example, a flavor profile of -3 is slightly sweet, one of -10 is extremely sweet, one of 2 is mildly
savory, and one of 8 is significantly savory.

You're given an array of these dishes and a target combined flavor profile. Write a function that returns the
best possible pairing of two dishes (the pairing with a total flavor profile that's closest to the target one).
Note that this pairing must include one sweet and one savory dish. You're also concerned about the dish
being too savory, so your pairing should never be more savory than the target flavor profile.

All dishes will have a positive or negative flavor profile; there are no dishes with a 0 value. For simplicity,
you can assume that there will be at most one best solution. If there isn't a valid solution, your function
should return ```[0, 0]``` . The returned array should be sorted, meaning the sweet dish should always come
first.

```
Sample Input:
dishes = [-3,-5,1,7]
target = 8
Sample Output:
[-3,7]  // 4 is the closest sum

Sample Input:
dishes = [2,5,-4,-7,12,100,-25]
target = -20
Sample Output:
[-25,5]  // gives the exact sum

Sample Input:
dishes = [3,5,7,2,6,8,1]
target = 10
Sample Output:
[0,0]  // there are no sweet dishes
```
## Optimized Approach-1
Since this problem is basically asking us to find the sum of pairs with the least difference from the target value, we can ```sort``` the array and work 
using ```left and right pointers```. This way, we can compare if the ```currentSum``` of pairs is greater or less than the ```target``` value and move 
the pointers accordingly. Additionally, to keep track of the pair closest to the target profile, we maintain a dictionary with difference between the sum of the 
current pair and target profile as the key: ```currentDiff``` and the actual pairs (```dishes[left] and dishes[right]```) as values.

1. If ```currentSum``` of pairs exceeds the target profile, it is not a valid pair as *"your pairing should never be more savory than your target
profile".*
2. The pair should contain one +ve and one -ve value in a sorted order, since *"the pairing must include one sweet and one savory dist".*

### Python Implementation:
```python
def sweetAndSavory(dishes, target):
    
    # sort the dishes in order to use left and right pointer comparison
    dishes.sort()  # O(nlogn)

    # dictionary to keep a track of the difference between each pair
    differences = {}

    # initialize left and right pointers
    left = 0
    right = len(dishes)-1

    # return an empty pair if the number of elements in the array is less than 1
    if len(dishes) <= 1:
        return [0,0]
    # return an empty pair if there are no sweet or savory dishes in the array
    if dishes[left] > 0 or dishes[right] < 0:
        return [0,0]

    # compare the sum of left and right pointer elements to the target to find the pair.
    while left < right:  # O(n)
        currentSum = dishes[left] + dishes[right]
        currentDiff = abs(target - currentSum)
        
        # move to the left to increase currentSum
        if currentSum < target:
            # only add a sweet-savory (-ve,+ve) pair to the dictionary
            if dishes[left] < 0 and dishes[right] > 0:
                differences[currentDiff] = [dishes[left],dishes[right]]
            left += 1
    
        # return sweet-savory pair if currentDiff is 0
        # if the pair is not sweet-savory, continue iteration by both the pointers
        elif currentSum == target:
            if dishes[left] < 0 and dishes[right] > 0:
                return [dishes[left], dishes[right]]
            left += 1
            right -= 1

        # in case currentSum > target, we do not add it to the dictionary as 
        # the pairing should never be more savory than the target flavour profile
        # move to the right to decrease currentSum
        else:
            right -= 1

    # return the pair with the least difference (key) in case the dictionary is not empty
    if differences != {}:
        return differences.get(min(differences))
    else:
        return [0,0]
```

### Complexity Analysis:
1. Time Complexity: O(nlogn) + O(n) : sorting + looping ~ ```O(nlogn)``` as given by the code.
2. Space Comlexity: ```O(n)``` due to the usage of a dictionary of maximum possible size 'n'.

## Optimized Approach-2 (Easier to understand)
Since we need 1 value each from sweet and savory, it is best to create a seperate ```sorted``` list for each of these flavour profiles. Sorting is necessary for
comparing the ```currentSum``` to the ```target``` and moving list pointers based on this comparison. We can keep track of the pair closest to the target using a 
variable ```bestDiff```.

### Python Implementation:
```python
def sweetAndSavory(dishes, target):

    # sort sweet and savory dishes by how sweet and savory they are.
    # Since -3 is less sweeter than -6, we need to sort using their magnitudes
    sweetDishes = sorted([dish for dish in dishes if dish < 0], key = abs)  # O(nlogn)
    savoryDishes = sorted([dish for dish in dishes if dish > 0])  # O(nlogn)

    # [0,0] to be returned if there are no sweet or savory dishes.
    bestPair = [0,0]
    bestDiff = float('inf')
    sweetIndex, savoryIndex = 0,0

    while sweetIndex < len(sweetDishes) and savoryIndex < len(savoryDishes):  # O(n)
        currentSum = sweetDishes[sweetIndex] + savoryDishes[savoryIndex]

        # only check for bestPair in case the currentSUm < target since
        # the pairing should never be more savory than the target flavour profile
        if currentSum <= target:
            currentDiff = target - currentSum
            if currentDiff < bestDiff:
                bestDiff = currentDiff
                bestPair = [sweetDishes[sweetIndex], savoryDishes[savoryIndex]]
            # since the sum is less than the target, we need to add a higher positive value
            # thus, we need to increment the savoryIndex
            savoryIndex += 1
        
        else:
            # increment to a higher negative value since currentSum > target and we need to
            # decrement currentSum to bring it closer to the target profile
            sweetIndex += 1

    return bestPair
```

### Complexity Analysis:
1. Time Complexity: O(nlogn) + O(n) : sorting + looping ~ ```O(nlogn)``` as given by the code.
2. Space Comlexity: ```O(n)``` due to usage of 2 lists which have 'n' elements combined.

## Key Takeaway
In case of questions where you need to return a list of different ```types``` of elements by ```comparing``` elements of one type to another, it is best to create
```seperate lists``` for each of them and use ```pointers``` for comparison.
