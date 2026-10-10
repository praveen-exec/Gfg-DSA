# Stock Span Problem

## Problem Statement

Given an array `arr` representing the daily stock prices, calculate the **span of the stock's price for each day**.

The span of a stock's price on a particular day is the maximum number of consecutive days, ending on that day, for which the stock price was less than or equal to the current day's price.

## Approach: Monotonic Stack

We use a **stack to store indices** of stock prices in decreasing order.

### Algorithm

1. Create an empty stack `s` and an empty vector `ans`.
2. Push index `0` into the stack and store `1` as the span of the first day.
3. For every index `i` from `1` to `n-1`:

   * Pop indices from the stack while the stack is not empty and `arr[s.top()] <= arr[i]`.
   * If the stack becomes empty, the span is `i + 1`.
   * Otherwise, the span is `i - s.top()`.
   * Store the calculated span in `ans`.
   * Push the current index `i` into the stack.
4. Return the answer vector.

## C++ Solution

```cpp
class Solution {
public:
    vector<int> calculateSpan(vector<int>& arr) {
        int span;
        stack<int> s;
        vector<int> ans;

        s.push(0);
        ans.push_back(1);

        for (int i = 1; i < arr.size(); i++) {
            while (!s.empty() && arr[s.top()] <= arr[i]) {
                s.pop();
            }

            span = s.empty() ? i + 1 : i - s.top();

            ans.push_back(span);
            s.push(i);
        }

        return ans;
    }
};
```

## Example

**Input:**

```text
arr = [100, 80, 60, 70, 60, 75, 85]
```

**Output:**

```text
[1, 1, 1, 2, 1, 4, 6]
```

## Dry Run

| Day (Index) | Price | Previous Greater Price | Span |
| ----------: | ----: | ---------------------: | ---: |
|           0 |   100 |                   None |    1 |
|           1 |    80 |                    100 |    1 |
|           2 |    60 |                     80 |    1 |
|           3 |    70 |                     80 |    2 |
|           4 |    60 |                     70 |    1 |
|           5 |    75 |                     80 |    4 |
|           6 |    85 |                    100 |    6 |

## Why Do We Store Indices?

We store indices instead of prices because the span is calculated using the distance between the current day and the previous greater price.

* If the stack is empty, `span = i + 1`.
* Otherwise, `span = i - s.top()`.

## Complexity Analysis

* **Time Complexity:** `O(n)` — Each index is pushed onto and popped from the stack at most once.
* **Space Complexity:** `O(n)` — The stack and answer vector may store up to `n` elements.

## Key Concept

The solution uses a **monotonic decreasing stack** to efficiently find the previous greater element for each stock price. Equal prices are also popped because the condition uses `<=`.

This avoids checking previous days one by one and makes the solution linear in time.
