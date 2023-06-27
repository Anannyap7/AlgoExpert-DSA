# Transpose Matrix

## Understanding the problem
Find the transpose of a 2D array (```matrix```). Basically, swap the rows and columsn of the original matrix to form the transpose matrix.

## Approach-1

Since transpose is simply the switching of rows and columns, create a new matrix with rows of the original matrix as columns and columns of the given matrix as rows of the new matrix. Now, all we have to do is add elements from the given matrix in the required places of the new 2d matrix to create a transpose. This can be done by transferring the elements of the given array to a 1d array for easier access and asigning.

### Algorithm:
1. Count the number of rows and columns in the given matrix
