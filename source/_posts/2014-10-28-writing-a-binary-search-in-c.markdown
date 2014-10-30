---
layout: post
title: "How to Write a Binary Search in C"
date: 2014-10-28 10:33
comments: true
categories: 
---

***First off, what is a binary search?*** A binary search, also known as a half-interval search, is an algorithm that finds a target number in a sorted sequence of numbers by comparing the target number to the middle number in the sequence. If the target number is smaller than the middle number, the entire top half of the sequence is ignored.

Let's break this down with an example. Let's say we're looking for the number 51 in this range of numbers

```
0 7 21 34 47 51 89 100 121 151 191 203 500 876 981 1000
```

There are 16 numbers in this sequence, and in computer programming, we begin counting sequences of numbers at 0, which makes the middle number in this sequence `121`. In this case, 51 is less than 121, so every number greater than 121, and 121 itself is ignored, making the sequence of numbers look like this:

```
0 7 21 34 47 51 89 100
```

Now that we have 8 numbers in the sequence, the middle number is `47`. Since 51 is a larger number, we can ignore all the numbers 47 on down, leaving us with `15 89 100`. Once again we take the middle number, `89`. 51 is smaller, so we ignore 89 and 100, leaving us with just 15. If our target number is 15, we have officially found our target.

##So how do we write this in C?

First things first, we need an array that holds all 16 numbers:

```c
int numbers[16] = {0, 7, 21, 34, 47, 51, 89, 100, 121, 151, 191, 203, 500, 876, 981, 1000};
```

Next we need to define our own search function:

```c
bool search(int value, int values[], int n)
{
 
  return false;
}
```

This function, called `search` takes in 3 arguments, the target number that you're looking for, `value`, the array that you're searching through, `values`, and the length of the array, `n`. This method also returns false (or zero) in the case of success.

Now that we have the function set up, some pseudo-code to get us thinking in the right way:

  1. Set up a base case (if there is only 1 item in the array) so that the recursive loop will quit appropriately 
  2. Create a variable to store the index of the middle number in the array
  3. Instantiate a new array from the correct half of the original array to search through
  4. Compare the target number to the middle number
  5. If the target number is greater than the middle number, ignore everything from the middle number down and store the larger numbers in a new array and vice versa
  5. Call search function to find the middle again....


###Set up a base case


```c
bool search(int value, int values[], int n)
{
  if (n < 2)
  {
    if (value == values[0])
    {
      return false;
    }
    else
    {
      return true;
    }

  }
  
}
  return false;
}
```

In this base case, the first if statement is checking to see if the size of the array `n`, is less than one, meaning is there only one item in this array? If that statement evaluates to true, the nested if/else statement will kick in. The nested if statement is checking to see if the target number `value` is the number in the array. If it is, the function will return false, if it isn't it will return true.


###Create a variable to store the index of the middle number in the array

The middle number in the array is going to be `n/2`. If `n` is 16, the middle number would be 8. If `n` is 15, the middle number would be 7.
```c
bool search(int value, int values[], int n)
{
  if (n < 2)
  {
    if (value == values[0])
    {
      return false;
    }
    else
    {
      return true;
    }
  }

  int middle = n/2;
  
}
  return false;
}
```

On line 15, we set up a variable `middle` to store the middle value.

###Instantiate a new array from the correct half of the original array to search through


```c
bool search(int value, int values[], int n)
{
  if (n == 1)
  {
    if (value == values[0])
    {
      return false;
    }
    else
    {
      return true;
    }
  }

  int middle = n/2;
  const int MAX = 65536;
  int half[MAX];
  
  return false;
}
```

On lines 16 we create a new constant, which stores the number 65536. This massive number will be useful to gaurantee that the search function can handle incredibly large arrays. On line 17 we create a new array called `half`, declare that it will store up to 65536 integers.


###Compare the target number to the middle number

We're going to need to set up more flow control based on the results of comparing `value` to the middle number in the array.


```c
bool search(int value, int values[], int n)
{
  if (n == 1)
  {
    if (value == values[0])
    {
      return false;
    }
    else
    {
      return true;
    }
  }

  int middle = n/2;
  const int MAX = 65536;
  int half[MAX];
  if (value > values[middle])
  {
  }
  else if (value < values[middle])
  {

  }
  else if (value == values[middle])
  {

  }
  
  return false;
}
```

Now we have our logic set up for either situation, if `value` (the number we're searching for) is either less than or greater than or equal to `values[middle]`, which is the number at the middle index of the `values` array.


###If the target number is greater than the middle number, ignore everything from the middle number down and store the larger numbers in a new array and vice versa

Once we're inside our if statements, we need to be able to select just half of the original array, by storing it in the new array we instatiated previously.

```c
bool search(int value, int values[], int n)
{
  if (n == 1)
  {
    if (value == values[0])
    {
      return false;
    }
    else
    {
      return true;
    }
  }

  int middle = n/2;
  const int MAX = 65536;
  int half[MAX];
  if (value > values[middle])
  {
    int new_size = n - middle;
    if (value > values[middle])
    {
      int new_size = n - middle -1;
      for (int i = 0, m = middle+1; i < new_size; i++, m++)
      {
        new_array[i] = values[m];
      }
    }
    return search(value, new_array, new_size);
  }
  else if (value < values[middle])
  {
    int new_size = n - middle;
    for (int i = 0, m = 0; i < middle; i++, m++)
    {
      new_array[i] = values[m]; 
    }
    return search(value, new_array, new_size);
  }
  else if (value == values[middle])
  {
    return true;
  }
  
  return false;
}
```

LINES 20-29
In the case that `value` is greater than `values[middle]`, we want to take the top half of the `values` array. We first create a `new_size` variable which stores the size of the top half of the array (n - middle -1). Then we iterate over the top half of the `values` array place the value of each index in the `half` array. This for-loop needs to incrementors, `i` which will iterate through `half`, and `m` which stores the index of the middle item in the array and iterates through `values`. Once we have our new array, we need to call our search function again. This is recursive implementation of a binary search, which means we're calling a function inside the definition of that function. Our function with return out when it hits the base case, aka when there is only one index left in the array.

And when `value` is less than `values[middle]`? We need to take the bottom half of the `values` array and place those values in an empty `half` array. Again, we have to call our search function.

An important part here is how we call the search function recursively on lines 29 and 37, `return search(value, new_array, new_size);`. We need to explicitly return the return value of those recursive calls so that they can move up the tree and the original search function can return the result.



