# Dynamic Programming


## Coin Change
### Top Down
- Time complexity: $$O(\text{Amount} \times \text{Types of Coins})$$
- Space complexity: $$O(\text{Amount})$$
```cpp
// Time complexity: O(amount*(coins.size()))
// Space complexity: O(amount)
int top_down(int remain, vector<int> &coin_list, vector<int> &memo)
{
    if (remain == 0)
    {
        return 0;
    }
    if (remain < 0)
    {
        return INT_MAX;
    }
    if (memo[remain] != -1)
    {
        return memo[remain];
    }

    int current_count = INT_MAX;
    for (int c : coin_list)
    {
        int res = top_down(remain - c, coin_list, memo);
        if (res != INT_MAX) // To prevent overflow at 1 + res
        {
            current_count = min(current_count, 1 + res);
        }
    }
    memo[remain] = current_count;
    return memo[remain];
}

int coinChange(vector<int> &coins, int amount)
{
    vector<int> memo(amount + 1, -1);
    memo[0] = 0;

    // If cannot be reached, memo[amount] is INT_MAX.
    // In that case, return -1
    return top_down(amount, coins, memo) == INT_MAX
               ? -1
               : top_down(amount, coins, memo);
}
```

