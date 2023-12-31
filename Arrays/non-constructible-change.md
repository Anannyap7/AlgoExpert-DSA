# Non-Constructible Change

## Understanding the problem

Given an array of positive integers representing the values of coins in your possession, write a
function that returns the minimum amount of change (the minimum sum of money) that you
cannot create. The given coins can have any positive integer value and aren't necessarily unique
(ie., you can have multiple coins of the same value).

For example, if you're given ```coins = [1, 2, 5]```, the minimum amount of change that you can't
create is ```4``` . If you're given no coins, the minimum amount of change that you can't create is ```1```.

## Optimized Approach

This is a tricky array problem. **So, in case you don't know how to approach such questions, it is best to sort the the array first and see what that looks like.**

### Example-based understanding:

The sample input is `[5, 7, 1, 1, 2, 3, 22]`. After sorting it, we get `[1, 1, 2, 3, 5, 7, 22]`. Let's walk through the sorted array and try to solve the problem.

```
[1, 1, 2, 3, 5, 7, 22]
 ^
```

Now we are at index 0, and we get `1`, so the amount of change we can create is `1`, we move on to the next integer.

```
[1, 1, 2, 3, 5, 7, 22]
    ^
```

The change we can create is `2 (1 + 1)`.

```
[1, 1, 2, 3, 5, 7, 22]
       ^
```

The change we can create are `4 (1 + 1 + 2)` and `3 (1 + 2)`. That means we are able to make any amount of change from `1` to `4`.

```
[1, 1, 2, 3, 5, 7, 22]
          ^
```

The change we can create are `7 (1 + 1 + 2 + 3)`, `6 (1 + 2 + 3)` and `5 (2 + 3)`, so we can make any amount of change from `1` to `7`.

```
[1, 1, 2, 3, 5, 7, 22]
             ^
```

The change we can create are `12 (1 + 1 + 2 + 3 + 5)`, `11 (1 + 2 + 3 + 5)`, `10 ( 2 + 3 + 5)`, `9 (1 + 1 + 2 + 5)` and `8(1 + 2 + 5)`, so now we can make any amount of change from `1` to `12`.

```
[1, 1, 2, 3, 5, 7, 22]
                ^
```

The change we can create are `19 (1 + 1 + 2 + 3 + 5 + 7)`, `18 (1 + 2 + 3 + 5 + 7)`, `17 (2 + 3 + 5 + 7)`, `16 (1 + 1 + 2 + 5 + 7)`, `15 (1 + 2 + 5 + 7)`, `14 (1 + 1 + 2 + 3 + 7)`, `13 (1 + 2 + 3 + 7)` so now we can make any amount of change from `1` to `19`. This also means, we can make previous amount of change plus the current integer, which is `7`. We were able to make `12`, add `7` to it, we get `19`; we were also able to make `11`, add `7` to it, we get `18`; `10` + `7` = `17`; `9` + `7` = `16`; ...

```
[1, 1, 2, 3, 5, 7, 22]
                   ^
```

The change we can create are `19` + `22` = `41`, `18` + `22` = `40`, `17` + `22` = `39`,
`16` + `22` = `38`, ... `1` + `22` = `23`, `22` and the previous amount of change: `19`...`1`. We are not able to make `20`, `21`. So we find the minimum amount of change we cannot create, which is `20`.

### Mathematical intuition inferred:
The above example clarifies that a **pattern** is formed. In case of an array of coins ```S=[1,...,n]```, imagine a variable ```C``` which represents the change that can be formed using the coins in the set.
Initially, ```C=0``` and the sorted array elements are added one by one to the change. As noticed in our example above, if ```C+1 >= Value of next element```, this means that we can make change from ```C``` till ```C + value of next element```.
If ```C+1 < Value of the next element```, this means that we can make change till C, but cannot make a change of ```C+1```.


Eg: ```S = [3,5,6,8]``` and ```C=0``` initially. Now, since C+1 (1) < 3, we cannot make the change ```1``` (C+1). So the non-cnstructible change here is 1.

### Algorithm:
1. Sort the input array ```coins``` in ascending order.
2. Initialize a variable ```change``` to keep track of the current change formed using the coins in the array.
3. Iterate through array elements and add them to the ```change``` variable.
4. For each iteration, check whether ```change + 1 >= coins[i+1]```. If the condition is satisfied, keep looping. If not, return ```change + 1``` as the non-constructible change.
5. In case of an empty array, return ```change + 1```.

### Python Implementation:

```python
def nonConstructibleChange(coins):
  coins.sort()  # O(nlogn)
  change = 0
  for i in range(len(coins)):  # O(n)
    if change+1 < coins[i+1]:
      return change + 1
    else:
      change += coins[i]
  return change+1
```

### Complexity Analysis:
1. Time Complexity: O(n) + O(nlogn) = ```O(nlogn)``` -> looping through given array + timsort.
2. Space Complexity: ```O(1)```

## Key Takeaway:
1. In case you don't know how to approach such questions, it is best to sort the the array first and see what that looks like.
2. Take small sized arrays and experiment with one of the numbers to see where certain conditions of the problem statement are broken. In this case [1,2,5] would violate the construction of change=4. That wont have been the case for any number less than 5 as the third element in this array. By doing this, try to play out the mathematical reasoning (and logical intuition behind why its working) through the equation that would fit.
