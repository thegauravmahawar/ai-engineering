# Linear Regression

Finding the best straight line through your data points to predict one thing based on another.

**The Goal:** If I know X, can I predict Y?

**Think of it like:** Drawing the "line of best fit" through scattered points on a graph - the line that gets as close as possible to all the points.

## Real-World Example: Predicting House Prices

Let's say you want to predict house prices based on size.

**Your Data (Training Data):**

| House | Size (sq ft) | Price ($) |
| ----- | ------------ | --------- |
| 1     | 800          | $180,000  |
| 2     | 1,000        | $220,000  |
| 3     | 1,200        | $260,000  |
| 4     | 1,500        | $310,000  |
| 5     | 1,800        | $350,000  |
| 6     | 2,000        | $390,000  |

**One Variable (X):** Size in square feet
**Thing to Predict (Y):** Price in dollars

```text
Price ($)
  400k |                    ⚫ (2000 sq ft, $390k)
       |                 ⚫ (1800 sq ft, $350k)
  300k |            ⚫ (1500 sq ft, $310k)
       |        ⚫ (1200 sq ft, $260k)
  200k |    ⚫ (1000 sq ft, $220k)
       | ⚫ (800 sq ft, $180k)
  100k |___________________________________________
         500   1000  1500  2000  Size (sq ft)
```
