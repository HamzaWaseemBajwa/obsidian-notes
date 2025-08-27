## NeetCode 150
### LC-125: Valid Palindrome 
#### Approach
##### Concepts
#two-pointers
###### Detail
- Initialize 2 pointers (left and right), one at start and one at end of string
- Advance left and retreat right, looping until left is less than right
- if left is not alpha numeric char, advance left only, breaking if condition is broken
- same for right
- when both chars valid, compare when lowered, return false if not same
- When loop finishes, return true
#### Code
```
class Solution:
    def isPalindrome(self, s: str) -> bool:
        if len(s) == 0 or len(s) == 1:
            return True
        l, r = 0, len(s) - 1
        while l < r:
            while l < r and not s[l].isalnum():
                l += 1
            while l < r and not s[r].isalnum():
                r -= 1
            if s[l].lower() != s[r].lower():
                return False
            l += 1
            r -= 1
        return True
```

#### Complexity
Time: O(n), Memory O(1)

### LC-309: Valid Palindrome
#### Approach
##### Concepts
#state-machine
##### Detail
Think of the simulation as a state machine. on each day, you start in one state, and can end in another (or remain in same state) based on what actions are available to you. For any current state, evaluate the best actions available to you in previous states, and record the results of those actions. Continue until end of simulation and return best possible result..

For this problem:
- You can be in one of 3 possible states at the start of each day: `CanBuy, CanSell or CoolDown`:
- Each State gives you the following choices:
	- `CanBuy: Buy or Wait`
	- `CanSell: Sell or Wait`
	- `CoolDown: Wait`
- Each choice has an associated end state and value change:
	- `CanBuy: Buy [{NextState = CanSell, NewValue = Value - PriceOnDay}, Wait [{NextState = CanBuy, NewValue = Value}]
	- `CanSell: Sell [{NextState = CoolDown, NewValue = Value + PriceOnDay, Wait [{NextState = CanSell, NewValue = Value}]`
	- `CoolDown: Wait [{NextState = CanBuy, NewValue = Value}]
- For each state, we will look at possible previous states, and select Best outcome:
	- `CanBuy: Max (Previous CanBuy, Cooldown)
	- `CanSell: Max (Previous CanSell, CanBuy - PriceOnDay)
	- `CoolDown: Previous CanSell + PriceOnDay
- On Day 0, we will start at the state `CanBuy`, will a value of `0`. The other state will be initialized at `-INF`, to ensure that they are not chosen as starting points for any valid sequence of actions 
- For each day, we will evaluate the best course of action for each end state, based on our best outcome from previously evaluated states
- At the end of the simulation, we will select the max value from either the `CanBuy` or `CoolDown` states. The `CanSell`, state is ignored, because its not possible to buy a stock on the last day (and end up in the `CanSell` state), and still have the BEST possible profit. 
#### Code
```
from math import inf
from typing import List
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        can_buy, can_sell, cool_down = 0, -inf, -inf
        for price in prices:
            new_can_buy = max(cool_down, can_buy)
            new_can_sell = max(can_sell, can_buy - price)
            new_cool_down = can_sell + price
            can_buy, can_sell, cool_down = new_can_buy, new_can_sell, new_cool_down
        return max(can_buy, cool_down)
```

# LC-400
