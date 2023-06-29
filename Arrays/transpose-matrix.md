# Transpose Matrix

## Understanding the problem
Find the transpose of a 2D array (```matrix```). Basically, swap the rows and columsn of the original matrix to form the transpose matrix.

## Approach-1
Since transpose is simply the switching of rows and columns, create a new matrix with rows of the original matrix as columns and columns of the given matrix as rows of the new matrix. Now, all we have to do is add elements from the given matrix in the required places of the new transposed matrix to create a transpose. This can be done by transferring the elements of the given array to a 1d array for easier access and asigning.

### Algorithm:
1. Count the number of ```rows``` and ```columns``` in the given matrix.
2. Initialize a 1D array and add the elements from the main array to ```array_1d```.
3. Create a ```transpose``` matrix filled with 0s with rows as columns and columns as rows. This swapping of rows and columns makes the resultant array a transpose matrix.
4. Iterate through the rows of the result matrix (using ```i```) and inside this loop initialize a new variable as ```k = i``` (this ensures that k points to an element from the 1D array to be added to the ith row of the resultant array).
5. Inside this loop, iterate through the columns of the new matrix (```j```)  and use the 1D array to insert the desired elements at the desired positions in the transpose/resultant matrix.
6. Increment ```k``` by ```cols``` to insert the row elements from the main array as columns in the new transpose array.
7. After looping through all the rows and columns, return the resultant matrix.

### Python Implementation:
```python
def transposeMatrix(matrix):

    # count the number of rows and columns in the given matrix
    rows = len(matrix)
    cols = len(matrix[0])

    # add the elements of the matrix to a 1d array
    array_1d = []
    for x in matrix:  # O(r*c)
        for y in x:
            array_1d.append(y)
    
    # form the resultant matrix using the rows as cols and cols as rows
    transpose = [[0 for i in range(rows)] for j in range(cols)]

    # selectively pick the desired numbers from the 1d array using 'k'
    # and add it in the correct position of the result matrix.
    for i in range(len(transpose)):  # O(c*r)
        k = i
        for j in range(len(transpose[i])):
            transpose[i][j] = array_1d[k]
            k += cols

    return transpose
```

### Complexity Analysis:
1. Time Complexity: O(r*c) + O(c*r) ~ ```O(r*c)``` where r = rows and c = columns.
2. Space Complexity: ```O(r*c)```

## Approach-2
Instead of going through the trouble of creating an 1D array from the original matrix and then accessing elements from this array, we can simply access the desired elements from the original matrix and place it on the transposed matrix. Since a ```transpose``` matrix is formed by simply swapping the rows and columns of the original matrix, we can add ```matrix[rows][cols]``` to the empty transpose matrix. Here, rows of the new array point to the columns of the original array and vice versa.

### Python Implementation:
```python
def transposeMatrix(matrix):
    transpose = []
    for col in range(len(matrix[0])): 
        newRow = []  # col in original matrix will act as rows in the new matrix
        for row in range(len(matrix)):  # O(r*c)
            newRow.append(matrix[row][col])  # columns elements are added to the row of the transpose matrix
        transpose.append(newRow)
    return transpose
```
### Complexity Analysis:
1. Time Complexity: O(r*c) + O(c*r) ~ ```O(r*c)``` where r = rows and c = columns.
2. Space Complexity: ```O(r*c)```

## Key Takeaway
When the solution is getting too complecated for an EASY level problem, try to think it from a different perspective from scratch.
