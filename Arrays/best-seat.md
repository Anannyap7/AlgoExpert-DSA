# Best Seat

## Understanding the problem

You walk into a theatre you're about to see a show in. The usher within the theatre walks you to your row
and mentions you're allowed to sit anywhere within the given row. Naturally you'd like to sit in the seat that
gives you the most space. You also would prefer this space to be evenly distributed on either side of you
(e.g. if there are three empty seats in a row, you would prefer to sit in the middle of those three seats).

Given the theatre row represented as an integer array, return the seat index of where you should sit. Ones
represent occupied seats and zeroes represent empty seats.

‘You may assume that someone is always sitting in the first and last seat of the row. Whenever there are
two equally good seats, you should sit in the seat with the lower index. If there is no seat to sit in, return -1.
The given array will always have a length of at least one and contain only ones and zeroes.

```
Sample Input:
seats = [1,0,1,0,0,0,1]
Sample Output:
4

Sample Input:
seats = [1,0,0,1,0,0,1]
Sample Output:
1

Sample Input:
seats = [1,1]
Sample Output:
-1
```

## Approach
As discussed earlier, when approaching a huge problem with multiple nuances, it is better to divide it into smaller sub-problems. 
This makes it easier to think of an approach by finding the solution to all the sub-problems individually.
Let's divide this problem into 3 parts:
* Preference of seat with most space -> means a seat surrounded by the most number of empty seats: ```seat surrounded by most number of continuous 0s```.
* Preference of middle seat in evenly distributed empty seats around you -> means occupying the middle seat in most conitnuous empty seats:
    * In case of odd number of empty seats - ```middle or mean position```.
    * In case of even number of empty seats - ```lower rounding of the middle or mean position```.
* Preference for a set with ```lower index``` in case of 2 equally good seats.

### Algorithm:
1. Initialize ```count``` (keeps track of the current count of 0s in the input array) and ```maxCount``` (to keep track of the most number of continuous 0s for most space) as 0.
2. Initialize the ```startPosition``` and ```endPosition``` for the range of continuous 0s as ```-1```. This is done if in case there are no empty seats in the array and we have to return -1.
3. Iterate through the array.
4. If the current element is 0, increment the ```count``` and compare it with ```maxCount``` in order to assign the maximum value among the two to maxCount. This is done to keep track of the range with the most 0s.
5. In case the current element is not 0, assign ```count=0``` to stop tracking once the subsequent 0s stop.
6. While assigning ```maxCount```, keep a track of the start and end positions of the range. The ```endPosition``` would be the position of the iterator at the end of the range. The ```startPosition`` would be the difference of the endPosition and the count of 0s + 1.
7. At the end of the loop, find the middlemost seat (```bestSeat```) by calculating the average or mean of the start and end positions.

### Python Implementation:
```python
def bestSeat(seats):

    count = 0
    maxCount = 0
    startPosition, endPosition = -1, -1

    # To find the start and end positions of the range of subsequent 0s
    for i in range(1, len(seats)):  # O(n)
        if seats[i] == 0:
            count += 1
        else:
            count = 0
        if maxCount < count:
            maxCount = count
            endPosition = i
            startPosition = (endPosition-count)+1

    # To find the middlemost seat:
    bestSeat = (startPosition + endPosition)//2
    return bestSeat
```

### Complexity Analysis:
1. Time Complexity: ```O(n)``` as we loop through the input array (of size 'n') once.
2. Space Complexity: ```O(1)``` as no additional data structure was used.

## Key Takeaway
* Divide a complex problem into sub-problems to make the thinking process easier.
* Try to think mathematically in case the problem contains many conditions and requires a single numerical output (not directly from the array elements).
