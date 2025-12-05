+++
date = '2025-12-05T17:06:09+05:30'
tags = ['til', 'Advent of Code']
title = 'Union of Intervals'
+++

[Today's AOC problem](https://adventofcode.com/2025/day/5) was a bit of a trap! The first part was so easy, that I got carried away and failed to notice the complexity till my solution slapped me on the face with an OOM!! It was so inefficient that 8 GB of heap space was not enough.

## The problem statement

Here are the fresh ingredient ID ranges

```
3-5
10-14
16-20
12-18
```

The ingredient IDs that these ranges consider to be fresh are 3, 4, 5, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, and 20. So, in this example, the fresh ingredient ID ranges consider a total of 14 ingredient IDs to be fresh.

## The (flawed) solution

My first solution was:
1. loop through the ranges
2. identify the `start` and `end` of the range
3. add all the numbers in between (inclusive of start and end) to a `Set`
4. fetch the size of the Set

```java
    private static void part2(String[] ranges) {
        Set<Long> freshIngredients = new HashSet<>();

        Arrays.stream(ranges).forEach(range -> {
            String[] boundaries = range.split("-");
            long start = Long.parseLong(boundaries[0]);
            long end = Long.parseLong(boundaries[1]);

            for (long i = start; i <= end; i++) {
                freshIngredients.add(i);
            }
        });
        
        System.out.println(freshIngredients.size());
    }
```

This worked pretty fine for the sample, but failed miserably for the actual input.

## The fix

After multiple (failed) attempts to come up with an algorithm that handled all the cases, I turned to reddit looking for some pointers and came across this [visualisation](https://www.reddit.com/r/adventofcode/comments/1penqq2/2025_day_5_part_2_visualization_for_the_sample/) that helped me solve this better.

{{< figure src="/images/union-of-intervals.png" >}}

So, this is how it works.

1. Collect all points of interests from the range

```
start: 3, 10, 16, 12
end (+1): 6, 15, 21, 19

pois (sorted): 3, 6, 10, 12, 15, 16, 19, 21
```

2. For each POI, check if it lies in any of the original range. If it does, count till the next POI.

```
3 - lies in 3 - 5 range
freshIngredientCount = 6 - 3 = 3

6 - out of range

10 - lies in 10 - 14 range
freshIngredientCount = 3 + (12 - 10) = 5

12 - lies in 12 - 18 range
freshIngredientCount = 5 + (15 - 12) = 8

15 - lies in 10 - 14 range
freshIngredientCount = 8 + (16 - 15) = 9

16 - lies in 16 - 20 range
freshIngredientCount = 9 + (19 - 16) = 12

19 - lies in 16 - 20 range
freshIngredientCount = 12 + (21 - 19) = 14

21 - out of range
```
