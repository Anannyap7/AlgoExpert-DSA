# Spiral Traverse

## Understanding the problem
Write a function that takes in an n x m two-dimensional array (that can be square-shaped when n == m)
and returns a one-dimensional array of all the array's elements in spiral order.

Spiral order starts at the top left corner of the two-dimensional array, goes to the right, and proceeds in a
spiral pattern all the way until every element has been visited.

```
Sample Input: "array": [
    [1, 2, 3, 4],
    [12, 13, 14, 5],
    [11, 16, 15, 6],
    [10, 9, 8, 7]
  ]
Sample Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]
```

## Approach
Think of this problem in the most brute-force way possible. Spiral traverse is basically traversing the along the perimeters of a rectangle. After successfully traversing through all sides of the rectangle, move all boundaries inward to form the next rectangle enclosed within the outermost rectangle and keep repeating this process till the starting boundary points for the row and column is behind the end boundary points.
1. Side-1 : Right horizontal traversal
2. Side-2 : vertical traversal
3. Side-3 : Left horizontal traversal
4. Side-4 : Up vertical traversal

### Algorithm:
1. Initialize pointers ```startRow``` and ```startCol``` at the beginning of the row and column. Initialize ```startCol``` and ```endCol``` at the end of the row and column respectively.
2. Create an empty 1D array ```result```.
3. Loop through until the startRow is greater than the endRow and the startCol is greater than the endCol.
4. Inside this loop,
   a. Right horizontal traversal: add elements from leftmost startCol till rightmost endCol for startRow
   b. Down vertical traversal: add elements from uppermost startRow till lowermost endRow for endCol
   c. Left horizontal traversal: add elements from rightmost endCol till leftmost startCol for endRow
   d. Up vertical traversal: add elements from lowermost endRow to the uppermost startRow for startCol
5. After 1 rectangular loop, increment ```startRow, startCol``` and decrement ```endRow, endCol``` to move onto the inner rectangle.
6. Once the loop ends, return the resulting array.

### Python Implementation - Iterative Solution:
```python
def spiralTraverse(array):

    startRow = 0
    endRow = len(array) - 1
    startCol = 0
    endCol = len(array[0]) - 1

    result = []

    while startRow <= endRow and startCol <= endCol:  # O(1)

        # The range in for loop is set such that there is no repetition of elements added
        
        # adding column elements from uppermost row(horizontal right traversal)
        for col in range(startCol, endCol+1):  # O(n)
            result.append(array[startRow][col])
        # to avoid duplication of elements
        if len(result) == len(array) * len(array[0]):
            break

        # adding row elements from rightmost column (vertical down tracersal)
        for row in range(startRow+1, endRow+1):  # O(n)
            result.append(array[row][endCol])
        if len(result) == len(array) * len(array[0]):
            break

        # adding column elements from lowermost row (horizontal left traversal)
        for col in range(endCol-1, startCol-1, -1):  # O(n)
            result.append(array[endRow][col])
        if len(result) == len(array) * len(array[0]):
            break

        # adding row elements from leftmost column (vertical up traversal)
        for row in range(endRow-1, startRow, -1):  # O(n)
            result.append(array[row][startCol])
        if len(result) == len(array) * len(array[0]):
            break

        startRow += 1
        endRow -= 1
        startCol += 1
        endCol -= 1

    return result
```

### Python Implementation - Recursive Solution:
```python
def spiralTraverse(array):
    
    result = []
    spiralFill(array, 0, len(array)-1, 0, len(array[0])-1, result)
    return result

def spiralFill(array, startRow, endRow, startCol, endCol, result):
    
    if startRow > endRow or startCol > endCol:
        return
        
    for col in range(startCol, endCol + 1):  # O(n)
        result.append(array[startRow][col])
    if len(result) == len(array) * len(array[0]):
        return
    
    for row in range(startRow + 1, endRow + 1):  # O(n)
        result.append(array[row][endCol])
    if len(result) == len(array) * len(array[0]):
        return
    
    for col in range(endCol - 1, startCol - 1, -1):  # O(n)
        result.append(array[endRow][col])
    if len(result) == len(array) * len(array[0]):
        return
    
    for row in range(endRow - 1, startRow, -1):  # O(n)
        result.append(array[row][startCol])
    if len(result) == len(array) * len(array[0]):
        return

    spiralFill(array, startRow + 1, endRow - 1, startCol + 1, endCol - 1, result)
```

### Complexity Analysis:
1. Time Complexity: O(n) + O(n) + O(n) + O(n) = 4 * O(n) ~ ```O(n)``` where 'n' is the total number of elements in the array.
2. Space Complexity: ```O(n)``` for the resulting array of size 'n'.

## Key Takeaway:
For traversal problems, look at one particular pattern that is repeating over and over to do the traversal. For spiral traversal, this meant realizing that we need to keep iteratively traversing the perimeter of the rectangle and update the rectangle being traversed to the inner rectangle after the outer ones are traversed successfully.
