# Week 14 — Arrays (Part 1)
## One Name, Many Values

**Deitel, Chapter 8** · Indexes · Loops · Finding maximum and minimum · Parallel arrays

## Learning goals

By the end of this lesson, you should be able to:

- Explain what an array is and why it is useful.
- Create an array with a fixed size or an initializer.
- Read and change an element using its index.
- Choose between `for` and `foreach` loops to work with array elements.
- Calculate a sum and average, find a maximum and minimum, and count matches.
- Use parallel arrays to keep related information together.

## 1. What is an array?

A **variable** stores a value. For example:

```csharp
double temperature = 95.75;
```

An **array** is a variable that stores multiple values of the **same type** under one name. Each stored value is called an **element**.

```csharp
double[] temps = { 95.75, 83.0, 97.624, 72.5, 86.25 };
```

Here, `temps` is an array containing five temperatures. It is easier to manage than five separate variables such as `temp1`, `temp2`, and `temp3`. A loop can visit every element in the array.

### Why use an array?

Suppose a program reads five daily temperatures, calculates their average, and displays each temperature above that average. Without an array, you would need five variables and repeated calculations and `if` statements. For 365 days, that approach becomes very long. An array lets the program use one name and a loop, no matter how many values it processes.

**Think about it:** Which parts of a five-temperature program would need to be repeated if you used separate variables?

## 2. Elements, indexes, and `Length`

Every array element has a numbered position called an **index**. In C#, indexes start at **0**. The index is an offset: the number of steps from the beginning of the array.

| Index | Element |
|---:|---:|
| 0 | `temps[0]` = 95.75 |
| 1 | `temps[1]` = 83.0 |
| 2 | `temps[2]` = 97.624 |
| 3 | `temps[3]` = 72.5 |
| 4 | `temps[4]` = 86.25 |

`temps.Length` tells you how many elements are in the array. This array has a length of 5, so its indexes go from 0 through 4.

### Check your understanding

| Question | Answer |
|---|---|
| What is `temps[2]`? | |
| What is `temps.Length`? | |
| What is the first index? | |
| What is the last index? | |
| What happens if you try to read `temps[5]`? | |

**Remember:** Valid indexes run from `0` through `Length - 1`. An index outside that range causes an `IndexOutOfRangeException` when the program runs.

## 3. Creating an array with a fixed size

Use `new` when you know how many elements the array should hold, but will provide the values later.

```csharp
dataType[] arrayName = new dataType[size];
```

For example, create space for five temperatures:

```csharp
const int TEMP_SIZE = 5;
double[] temps = new double[TEMP_SIZE];
```

A named constant gives the size a clear name. You can use the same pattern for other types:

```csharp
const int GRADE_SIZE = 6;
int[] grades = new int[GRADE_SIZE];

char[] codes = new char[4];
```

Here, `4` directly sets the size of the `codes` array. You could also put the size in a named constant:

```csharp
const int CODE_SIZE = 4;
char[] codes = new char[CODE_SIZE];
```

Both versions create an array with four elements, at indexes 0 through 3. A named constant is helpful when the size needs a meaningful name or is used in several places. If you use the size only once, writing the number directly is also fine.

When an array is created with `new`, its elements start with the default value for their type:

| Element type | Default value |
|---|---|
| `int`, `double`, `decimal` | `0` (or the equivalent numeric zero) |
| `bool` | `false` |
| `char` | `\0` (the null character) |
| `string` | `null` (no string value) |

### Your turn

1. What values are initially stored in `grades`?
2. What values are initially stored in `codes`?
3. Declare a constant for an array size of 9, then create an array named `prices` that holds 9 `double` values.

## 4. Reading and changing one element

Use the array name and an index in square brackets to access one element:

```csharp
arrayName[index]
```

The index must be between `0` and `Length - 1`. You can read an element, assign it a value, or use it in an expression.

```csharp
temps[2] = 95.75;              // Store 95.75 at index 2
temps[0] = temps[2] - 11.0;   // Read index 2; store the result at index 0

int i = 2;
temps[i + 1] = temps[i] + 1;  // Uses indexes 3 and 2
```

Assume these statements run in order. What values are in the following elements afterward?

| Element | Value |
|---|---:|
| `temps[0]` | |
| `temps[2]` | |
| `temps[3]` | |

**Careful:** An out-of-range index may compile successfully, but the program throws an `IndexOutOfRangeException` when it tries to access that element.

## 5. Creating an array with an initializer

When you already know the values, an **initializer** creates the array and fills it in one step:

```csharp
dataType[] arrayName = { value0, value1, value2 };
```

C# determines the array's length from the number of values you provide.

```csharp
int[] numbers = { 3, 5, 8, 90, 22, 412, 90 };
char[] letters = { 'a', 'Y', 'Z', 'g' };
string[] groceries = { "bananas", "soup", "cabbage" };
```

### Your turn

What are the values of `numbers[3]`, `numbers.Length`, and `letters[letters.Length - 1]`?

## 6. Using a `for` loop with an array

A `for` loop is useful when you need the index as well as the element. This common loop visits every valid index:

```csharp
for (int i = 0; i < arrayName.Length; i++)
{
    // Use arrayName[i]
}
```

The condition is `i < arrayName.Length`, not `i <= arrayName.Length`. The last valid index is one less than the length.

### Example: read, sum, and display five temperatures

We can organize the work into three steps:

1. **Input:** read each temperature into the array.
2. **Process:** add the elements and calculate the average.
3. **Output:** display the temperatures, sum, and average.

```csharp
const int TEMP_SIZE = 5;
double[] temps = new double[TEMP_SIZE];

// Input: fill the array
for (int i = 0; i < temps.Length; i++)
{
    Console.Write($"Enter temperature {i + 1}: ");
    temps[i] = double.Parse(Console.ReadLine());
}

// Process: calculate the sum and average
double sum = 0;
for (int i = 0; i < temps.Length; i++)
{
    sum += temps[i];
}
double average = sum / temps.Length;

// Output: display the values
Console.Write("You entered: ");
for (int i = 0; i < temps.Length; i++)
{
    Console.Write($"{temps[i]}  ");
}
Console.WriteLine();
Console.WriteLine($"Sum: {sum}   Average: {average:F2}");
```

`F2` formats the average with two digits after the decimal point.

### Sample run

```text
Enter temperature 1: 95.75
Enter temperature 2: 83
Enter temperature 3: 97.624
Enter temperature 4: 72.5
Enter temperature 5: 86.25
You entered: 95.75  83  97.624  72.5  86.25
Sum: 435.124   Average: 87.02
```

### Trace the sum loop

At each pass, `sum` increases by the current array element. Fill in the running total.

| `i` | `temps[i]` | `sum` after this pass |
|---:|---:|---:|
| 0 | 95.75 | |
| 1 | 83.0 | |
| 2 | 97.624 | |
| 3 | 72.5 | |
| 4 | 86.25 | |

### Your turn: temperatures above average

Add a loop that displays only the temperatures greater than `average`.

## 7. Finding the largest and smallest values

To find the maximum and minimum, start by treating the first array element as both values. Then compare each remaining element with them.

```csharp
double max = temps[0];
double min = temps[0];

for (int i = 1; i < temps.Length; i++)
{
    if (temps[i] > max)
    {
        max = temps[i];
    }

    if (temps[i] < min)
    {
        min = temps[i];
    }
}

Console.WriteLine($"Highest: {max}   Lowest: {min}");
```

For the sample temperatures, this displays:

```text
Highest: 97.624   Lowest: 72.5
```

The loop starts at index 1 because index 0 was already used to initialize `max` and `min`. Starting at 0 would also work, but it would compare the first element with itself unnecessarily. Starting `max` and `min` at 0 is not a sound general approach: the values might all be negative or all greater than zero.

### Trace the search

| Step | `temps[i]` | `max` after comparison | `min` after comparison |
|---|---:|---:|---:|
| Start | 95.75 | 95.75 | 95.75 |
| `i = 1` | 83.0 | | |
| `i = 2` | 97.624 | | |
| `i = 3` | 72.5 | | |
| `i = 4` | 86.25 | | |

## 8. Choosing between `for` and `foreach`

Both loops can visit every element in an array. The difference is what the loop gives you:

- A `for` loop gives you an **index** (a position number), such as `i`.
- A `foreach` loop gives you each **value** in order, without an index.

### Use `for` when you need the position or want to change elements

Because `for` gives you the index, you can use it to display an element's position:

```csharp
for (int i = 0; i < temps.Length; i++)
{
    Console.WriteLine($"Day {i + 1}: {temps[i]}");
}
```

Here, `i` identifies the array position. `i + 1` displays the day as 1 through 5, even though the indexes are 0 through 4.

Use `for` when you need to change the values stored in the array:

```csharp
for (int i = 0; i < temps.Length; i++)
{
    temps[i] = 0;
}
```

This sets each array element to zero. The index lets the program say exactly which element to change.

### Use `foreach` when you only need each value

A `foreach` loop is a simpler choice when you want to read every value and do not need its position. For example, display all the temperatures:

```csharp
foreach (double temp in temps)
{
    Console.WriteLine(temp);
}
```

Or add them together:

```csharp
double total = 0;
foreach (double temp in temps)
{
    total += temp;
}
```

The variable `temp` holds the current value for that pass through the loop. It is not an index. Assigning a new value to `temp` does not change the value stored in the array.

### A quick way to choose

Ask yourself: **Do I need to know the element's position, or change the element in the array?** If yes, use `for`. If you only need to read each value, `foreach` is usually easier.

| `for` | `foreach` |
|---|---|
| Gives you an index, such as `i` | Gives you each value directly |
| Use when position matters or you need to change array elements | Use when reading, summing, counting, or displaying values |
| You control which indexes the loop visits | Visits each element in order |

## 9. Counting matching values

An array can hold characters, too. This example counts the letters `o` and `n` in the word `goodmorning`, with one letter stored in each array element.

```csharp
char[] greeting = { 'g', 'o', 'o', 'd', 'm', 'o', 'r', 'n', 'i', 'n', 'g' };
int countO = 0;
int countN = 0;

for (int i = 0; i < greeting.Length; i++)
{
    if (greeting[i] == 'o')
    {
        countO++;
    }
    else if (greeting[i] == 'n')
    {
        countN++;
    }
}

Console.WriteLine($"o appears {countO} times, n appears {countN} times");
```

Output:

```text
o appears 3 times, n appears 2 times
```

**Think about it:** Could this loop use `foreach`? Yes. The code only reads each character and does not need its index.

## 10. Formatting array values in rows

The index can also help control output layout. This example prints nine prices, three per row:

```csharp
double[] prices = { 10.95, 16.3, 12.15, 8.22, 15.9, 26.22, 13.54, 6.45, 17.59 };

for (int i = 0; i < prices.Length; i++)
{
    Console.Write($"{prices[i],8:F2}");

    if ((i + 1) % 3 == 0)
    {
        Console.WriteLine();
    }
}
```

Output:

```text
   10.95   16.30   12.15
    8.22   15.90   26.22
   13.54    6.45   17.59
```

`i` starts at 0, but the first price is item 1. So `(i + 1) % 3 == 0` is true after every third price: indexes 2, 5, and 8.

**Think about it:** What would happen if the condition used `i % 3 == 0` instead?

## 11. Parallel arrays

Sometimes each item has several pieces of information. **Parallel arrays** store those pieces in separate arrays, using the same index for information about the same item.

For example, `prices[3]`, `quantities[3]`, and `costs[3]` all describe item 4 (index 3). The arrays must have the same length and stay in the same order.

This program reads the price and quantity for five items, calculates each item's cost, then displays a table:

```csharp
const int ITEM_COUNT = 5;
double[] prices = new double[ITEM_COUNT];
int[] quantities = new int[ITEM_COUNT];
double[] costs = new double[ITEM_COUNT];

for (int i = 0; i < ITEM_COUNT; i++)
{
    Console.Write("Enter item price: ");
    prices[i] = double.Parse(Console.ReadLine());

    Console.Write("Enter quantity purchased: ");
    quantities[i] = int.Parse(Console.ReadLine());

    costs[i] = prices[i] * quantities[i];
}

Console.WriteLine($"\n{"Price",8}{"Quantity",10}{"Cost",14}");
for (int i = 0; i < ITEM_COUNT; i++)
{
    Console.WriteLine($"{prices[i],8:F2}{quantities[i],10}{costs[i],14:C}");
}
```

### Sample run

```text
Enter item price: 19.99
Enter quantity purchased: 2000
Enter item price: .25
Enter quantity purchased: 500
Enter item price: 12.78
Enter quantity purchased: 50
Enter item price: 55.99
Enter quantity purchased: 12
Enter item price: 1.75
Enter quantity purchased: 250

   Price  Quantity          Cost
   19.99      2000    $39,980.00
    0.25       500       $125.00
   12.78        50       $639.00
   55.99        12       $671.88
    1.75       250       $437.50
```

### Your turn

Complete the cost column, then add a loop that calculates and displays the total cost of all five items.

| `i` | `prices[i]` | `quantities[i]` | `costs[i]` |
|---:|---:|---:|---:|
| 0 | 19.99 | 2000 | |
| 1 | 0.25 | 500 | |
| 2 | 12.78 | 50 | |
| 3 | 55.99 | 12 | |
| 4 | 1.75 | 250 | |

## 12. Common array mistakes

| Mistake | Example | What happens |
|---|---|---|
| Letting the index reach the length | `i <= temps.Length` | Eventually tries an invalid index and throws `IndexOutOfRangeException` |
| Starting at 1 when you mean to visit every element | `for (int i = 1; ...)` | Skips `temps[0]` |
| Declaring an array but never creating it | `double[] temps; temps[0] = 5;` | Does not compile because `temps` has no array assigned to it |
| Putting the size in the type brackets | `double[5] temps;` | Syntax error; use `new double[5]` |
| Treating `Length` like a method | `temps.Length()` | Compile error; use `temps.Length` |
| Printing the array name to display its contents | `Console.WriteLine(temps);` | Prints the array's type name, not its elements |
| Trying to change an array element through a `foreach` variable | `foreach (double t in temps) t = 0;` | Compile error; use a `for` loop and assign to `temps[i]` |

## Lesson summary

- An **array** stores many values of the same type under one name.
- Each value is an **element**. Its index starts at 0.
- For an array of length `Length`, valid indexes are `0` through `Length - 1`.
- Use `new dataType[size]` to create an array with a set size. Use `{ ... }` to create and fill an array with known values.
- Use `for (int i = 0; i < array.Length; i++)` when you need the index or need to change elements.
- Use `foreach` when you need to read every value and do not need its index.
- To find a maximum or minimum, initialize it from an actual array element.
- Parallel arrays use matching indexes to keep related values together.
