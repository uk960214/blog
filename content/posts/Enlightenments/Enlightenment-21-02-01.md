---
title: "Enlightenment of the Day 21.02.01"
categories: ["Enlightenment"]
date: 2021-02-01T21:03:03+09:00
tags: ["CS50", "C", "Algorithm", "Bubble Sort", "Recursion", "Tideman"]
draft: false
---

# Sorting and Locking Pairs for Tideman (CS50 Problem Set 3)

### The Original Problem Explained ([Tideman](https://cs50.harvard.edu/x/2021/psets/3/tideman/))

----

## The Problem Encountered

Although with the test sets provided with the example seemed to work just fine with the code I originally wrote, when graded, it showed that my code didn't lock the pairs correctly (meaning it didn't lock what it was supposed to, and locked pairs where it should have skipped).

## 1. Sorting Problem

**Instructions**

    Complete the sort_pairs function.

    The function should sort the pairs array in decreasing order of strength of victory, where strength of victory is defined to be the number of voters who prefer the preferred candidate. If multiple pairs have the same strength of victory, you may assume that the order does not matter.

**My Original Code**

``` C
void sort_pairs(void)
{
    // Assign variable to hold values while swapping pairs
    pair temp;
    int swaps;

    // Swap and sort until no more swap is needed
    do
    {
        swaps = 0;
        for (int i = 0; i < pair_count; i++)
        {
            // Swap places if i is smaller than i + 1
            if (preferences[pairs[i].winner][pairs[i].loser] 
                < preferences[pairs[i + 1].winner][pairs[i + 1].loser])
            {
                temp = pairs[i];
                pairs[i] = pairs[i + 1];
                pairs[i + 1] = temp;
                swaps ++;
            }
        }
    }
    while (swaps != 0);
    return;
}
```

When I ran the code with a larger test set (provided by [this link](https://gist.github.com/nicknapoli82/2379634e87f24399ed1ed12c4c2e8c9a#file-tideman_test-txt)), I found out that my problem started here. Strangely, the first pair to be locked was `[Null] => [a]`, which was supposed to be impossible.

The problem was with the loop.
``` C
for (int i = 0; i < pair_count; i++)
```
Considering that in this loop, in the course of bubble sort, `pairs[i]` is compared and swapped with `pairs[i + 1]` With the length of the loop as pair_count, in the last loop, the last pair would have been swapped with a non-existent pair. 

Therefore, the length of the loop should be modified to following.

```C
for (int i = 0; i < pair_count - 1; i++)
```

However this was not enough to solve the problem.

## 2. Checking for Possible Cycle

**Instruction**

    Complete the lock_pairs function.

    The function should create the locked graph, adding all edges in decreasing order of victory strength so long as the edge would not create a cycle.

**My Original Code**

```C
// Check if locking this pair creates a cycle
bool check_cycle(int loser, int init)
{
    // If the inital value becomes a loser at some point, return true
    if (loser == init)
    {
        return true;
    }

    // Check all candidates, if the loser input is winner to any other candidate
    for (int i = 0; i < candidate_count; i++)
    {
        if (locked[loser][i])
        {
            check_cycle(i, init)
        }
    }
    return false;
}

// Lock pairs into the candidate graph in order, without creating cycles
void lock_pairs(void)
{
    for (int i = 0; i < pair_count; i++)
    {
        // Only lock pairs if check cycle returns true
        if (check_cycle(pairs[i].loser, pairs[i].winner) == false)
        {
            locked[pairs[i].winner][pairs[i].loser] = true;
        }
    }
    return;
}
```

Using the same test sets as above, the problem was found while locking the following pairs. If [f] => [a] is locked, it creates a cycle. Therfore, the check_cycle function should return true, and this pair should not be locked.

    if locked pairs are:

    [b] => [h]

    [b] => [f]

    [a] => [b]

    and checking cycle for: [f] => [a]

In the code I wrote, the loop was processed as following.

`Step 1 >` 

    check_cycle(a, f)
    a != f
    a => b exists

`Step 2 >`

    check_cycle(b, f)
    b != f
    b => h exists

`Step 3 >` 

    check_cycle(h, f)
    h != f
    h has not been locked as winner
    return false, and the whole check cycle function has ended
    [f] => [a] is locked

As seen above, although b is winner to both h and f, before checking if the pair to f creates a cycle, the loop ends because after checking for h, `false` is returned for the whole function.

In order to fix this early termination of the check_cycle function, the code needs to be modified as follows.

``` C
// Check if locking this pair creates a cycle
bool check_cycle(int loser, int init)
{
    // If the inital value becomes a loser at some point, return true
    if (loser == init)
    {
        return true;
    }

    // Check all candidates, if the loser input is winner to any other candidate
    for (int i = 0; i < candidate_count; i++)
    {
        if (locked[loser][i])
        {
            // If so, recursively check for cycle, until ultimately, check_cycle reaches true or false
            if (check_cycle(i, init))
            {
                return true;
            }
        }
    }
    return false;
}

// Lock pairs into the candidate graph in order, without creating cycles
void lock_pairs(void)
{
    for (int i = 0; i < pair_count; i++)
    {
        // Only lock pairs if check cycle returns true
        if (check_cycle(pairs[i].loser, pairs[i].winner) == false)
        {
            locked[pairs[i].winner][pairs[i].loser] = true;
        }
    }
    return;
}
```

By adding the conditional statement, the loop proceeds if the recursive function returns false, not terminating the original loop.

`Step 1 >` 

    check_cycle(a, f)
    a != f
    a => b

`Step 2 >`

    check_cycle(b, f)
    b != f
    b => h

`Step 3 >` 

    check_cycle(h, f)
    h != f
    h has not been locked as winner
    return false, but return to loop in Step 2

`Step 4 >` 

    continuing check_cycle(b, f)
    b => f

`Step 5 >` 

    check_cycle(f, f)
    f == f
    return true
    [f] => [a] is not locked