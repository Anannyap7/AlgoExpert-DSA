# Tournament Winner

## Understanding the problem

There's an algorithms tournament taking place in which teams of programmers compete against each other to solve
algorithmic problems as fast as possible. Teams compete in a round robin, where each team faces off against all other teams.
Only two teams compete against each other at a time, and for each competition, one team is designated the home team, while
the other team is the away team. In each competition there's always one winner and one loser; there are no ties. A team
receives 3 points if it wins and 0 points if it loses. The winner of the tournament is the team that receives the most amount of
points.

Given an array of pairs representing the teams that have competed against each other and an array containing the results of
each competition, write a function that returns the winner of the tournament. The input arrays are named ```competitions```
and ```results```, respectively. The competitions array has elements in the form of ```[homeTeam, awayTeam]``` , where each
team is a string of at most 30 characters representing the name of the team. The results array contains information about
the winner of each corresponding competition in the ```competitions``` array. Specifically, ```results[i]``` denotes the winner
of ```competiitions[i]``` ,where a ```1``` inthe ```results``` array means that the home team in the corresponding competition
won and ```0``` means that the away team won.

It's guaranteed that exactly one team will win the tournament and that each team will compete against all other teams exactly
once. It's also guaranteed that the tournament will always have at least two teams.

## Optimized Approach

The solution to this problem is quite straightforward. **How would you keep a track of the points earned by each team?** Using key-value pairs in a dictionary.
**How to avoid the brute-force approach of using nested for-loops to access teams in the ```competitions``` array?** Look closely and you will discover that the length of ```results``` array is equal to the outer length of the ```competitions``` array.
This means that by simply looping through the results array, we can access each round in the competitions array. **How to access a team in each round?** Due to the fixed number of teams in each round, (2 teams) simply access the winning team using their indices instead of creating a new iterator for the same. 
**How to access the indices of the winning team?** The elements in the results array are the complement of actual indices of the winning teams in the competitons array.
So, by simply taking the complement of these values, we can access the winner using the same iterator of the results array. In this way, we can ensure a time complexity of **O(n)**.

### Algorithm:
1. Create an empty dictionary: ```dict = {}```
2. Iterate through the ```results``` array.
3. Initialize a variable indicating the winner of each round inside the loop (```winner```)
4. Check if the winner is already present in the dictionary. If not, then add the winner as a key-value pair of winningTeam : 3 (points earned). If yes, then simply add 3 points to the value of the winning team.
5. In order to find the key correspinding to the maximum value, create seperate lists of ```keys``` and ```values``` and access the index of the element from ```keys``` which corresponds to the maximum value in ```values```.
6. Return the winning team.

### Python Implementation

```python
def tournamentWinner(competitions, results):
  # take complement of values in the results array
  results = [abs(x-1) for x in results]

  dict = {}
  
  for i in range(len(results)):  # O(n)
    winner = competitions[i][results[i]]
    if winner not in dict:  # O(1) - searching in hashtable/dictionary
      dict[winner] = 3
    else:
      dict[winner] += 3

  # return key with max value
  keys = list(dict.keys())
  values = list(dict.values())
  return keys[values.index(max(values))]
```

### Complexity Analysis:
1. Time Complexity: O(n) + O(1) = ```O(n)``` where 'n' is the size of the ```results``` array.
2. Space Complexity: space occupied by dict + list of keys + list of values = O(n) + O(n) + O(n) = O(3n) ~ ```O(n)```.

## Key Takeaway
Whenever working with score maintenance problem of some sort, think of using hashmaps/dictionaries.
