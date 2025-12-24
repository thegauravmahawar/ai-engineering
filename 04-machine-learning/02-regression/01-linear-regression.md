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

## The Mathematical Idea (Simple!)

Linear regression finds the equation of a line:

```text
y = mx + b
```

Or in machine learning notation:

```text
ŷ = wx + b
```

Where:

- ŷ (y-hat) = Predicted value (what we're trying to guess)
- x = Input feature (what we know)
- w = Weight/Slope (how much y changes when x increases)
- b = Bias/Intercept (starting point when x = 0)

For example, after linear regression analyzes our 6 houses, it might find:

```text
Price = 200 × Size + 20,000
```

Let's break this down:

**w (slope) = 200**

- Meaning: Each additional square foot adds $200 to the price
- "Houses cost $200 per square foot"

**b (intercept) = 20,000**

- Meaning: Base price before considering size
- "There's a $20,000 starting price"

## Making Predictions

Example 1: A 1,300 sq ft house

```text
Price = 200 × 1,300 + 20,000
Price = 260,000 + 20,000
Price = $280,000
```

Example 2: A 2,500 sq ft house

```
Price = 200 × 2,500 + 20,000
Price = 500,000 + 20,000
Price = $520,000
```

## How does it find the best line?

For each training example:

- Use the line to predict the price
- Compare prediction to actual price
- Calculate the error (difference)
- Square the error (to make negatives positive)
- Add up all squared errors

The algorithm adjusts the line to minimize total error. This is called **"minimizing the cost function"** or **"least squares method"**.

## Visual Example

**Start:** Algorithm makes a random guess for the line

```text
Price = 50 × Size + 100,000  (random starting line)
```

**Test it on first house (800 sq ft, actual price $180k):**

- Predicted: 50 × 800 + 100,000 = $140,000
- Actual: $180,000
- Error: $40,000 too low!

**Test it on all houses - errors are huge!**

**Adjustment:** Algorithm says "my line is too flat, need steeper slope!"

```text
Price = 150 × Size + 50,000  (adjusted)
```

**Test again - still errors, but smaller**

**Keep adjusting until errors are minimized:**

```text
Price = 200 × Size + 20,000  (final best line)
```
