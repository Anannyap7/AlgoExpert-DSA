# Array of Products

## Understanding the Problem
Write a function that takes in a non-empty array of integers and returns an array of the same length, where
each element in the output array is equal to the product of every other number in the input array.

In other words, the value at ```output[i]``` is equal to the product of every number in the input array other
than ```dinput[i]``` .

**[Note that you're expected to solve this problem without using division.]** - This is an easy method with O(n) time complexity

```
Sample Input:
array = [5, 1, 4, 2]

Sample Output:
[8, 40, 10, 20]
// 8 = 1 x 4 x 2
// 40 = 5 x 2 x 4
// 10 = 5 x 1 x 2
// 20 = 5 x 1 x 4
```

## Approach-1: Brute-Force Approach
This approach is the first that comes to our mind. Take two pointers/iterators and while the multiply all elements at the second pointer which are not in the same
position as the first pointer. However, this method results in O(N^2) time complexity as for every element (n), the second pointer iterates through the array (n-1) times.

### Python Implementation:
```python
def arrayOfProducts(array):

    result = []

    for i in range(len(array)):
        product = 1
        for j in range(len(array)):
            if i != j:
                product *= array[j]
            else:
                continue
        result.append(product)

    return result
```

### Complexity Analysis:
1. Time Complexity: ```O(n^2)``` where 'n' is the size of the input array.
2. Space Complexity: ```O(n)``` where 'n' is the size of the output array.

## Approach-2: Optimized
For each index in the input array, try calculating the product of every element to the left and the product of every element to the right with the help of 2 
seperate loops to ensure that the array is looped through only once.
Add these products to their respective left and right arrays. Then multiply these arrays to get the resulting array of products.

### Python Implementation-1:
* left products (forward traverse): ```leftProd```
* right products (backward traverse): rightProd
* result[i] = leftProd[i] * rightProd[i]
```python
def arrayOfProducts(array):

    leftProd = [0]*len(array)
    rightProd = [0]*len(array)
    
    product = 1
    # For products to the left of the current element: forward traverse
    for i in range(len(array)):
        # first insert the left product of elements for the current element
        # then find the new left product for the next element of the array
        # move onto the next element (= current element).
        leftProd[i] = product
        product *= array[i]

    product = 1
    # for products to the right of the current element: backward traverse
    for i in range(len(array)-1, -1, -1):
        rightProd[i] = product
        product *= array[i]

    # multiply values in leftProd to values in the corresponding index in
    # rightProd to obtain the final array of products
    result = []
    for i in range(len(array)):
        result.append(leftProd[i]*rightProd[i])

    return result
```

### Python Implementation-2:
* left products in initial ```result``` array
* right product at ```i``` multiplied with the ith element of the resulting array (of left products)
* more direct approach
```python
def arrayOfProducts(array):

    result = [1]*len(array)

    leftProduct = 1
    for i in range(len(array)):
        result[i] = leftProduct
        leftProduct *= array[i]

    rightProduct = 1
    for i in reversed(range(len(array))):
        result[i] *= rightProduct
        rightProduct *= array[i]

    return result
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` ~ O(3n) = O(n) + O(n) + O(n) -> as wee iterate through the array once for the left, once for the right product and once for the resulting array.
2. Space Complexity: ```O(n)``` where 'n' is the size of the output array.

## Key Takeaway
Split a problem in small sub-problems in order to simplify the solution like we did here. We split the products in the left and right direction and ultimately combined them both.
So, this is better than trying to tackle the problem directly which resulted in O(n^2) time complexity.
