+++
date = '2025-12-03T17:44:29+05:30'
tags = ['til', 'Advent of Code']
title = 'Substring vs Subsequence'
+++

It is that time of the year when everyone eagerly awaits to solve the AOC problem everyday. Today's [problem](https://adventofcode.com/2025/day/3) was pretty easy to understand as a human, but it is a bit hard to come up with an algorithm to solve it efficiently.

Given an input `234234234234278` we need to come up with the largest 12 digit number, without changing the order of the digits `434234234278` in this case. In other words, we need to derive a *subsequence* from the input.

Substrings are very commonly used in application development. Subsequence is not. 

While both of them need to *preserve the order* of characters that appear in the input string, Substrings are actually a subset of subsequence that satisfies the condition that, the elements needs to be *continuous*. In a subsequence, we can drop elements in between, while retaining the order of appearance.

For example, Here are some of the valid subsequences of `234234234234278`
|subsequence|input|
|---|---|
|244|`2`~~3~~`4`~~23~~`4`~~234234278~~|
|342|~~2~~`342`~~34234234278~~|
|478|~~2323~~`4`~~2342342~~`78`|
|242|~~234~~`2`~~3423~~`4`~~234~~`2`~~78~~|
|278|~~234234234234~~`278`|

Out of these subsequences, here are the ones that are substrings of the given input

|substring|input|
|---|---|
|342|~~2342~~`342`~~34234278~~|
|278|~~234234234234~~`278`|
