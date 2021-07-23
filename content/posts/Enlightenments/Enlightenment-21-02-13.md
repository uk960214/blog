---
title: "Enlightenment of the Day 21.02.13"
categories: ["Enlightenment"]
date: 2021-02-15T22:07:15+09:00
tags: ["C", "Problem-Solving", "Datatype", "Image Filter", "Floats"]
draft: false
---

# Handling Edge Cases in Image Filtering / Understanding Rounding of Floats in C

## 1. Handling Edge Cases in Image Filtering

## Problem
Using C, create a program that apllies different filters on images.
Each pixel is stored in a two dimensional array.

## Obsctacle
When filtering the image with 'blur' and 'border' filter, for each pixel in the image, certain values of all surrounding pixels needs to be reached. However, the number of surrounding pixels vary depending on the location of the pixel, more precisely, depending on whether the pixel is in the corner, edge or the middle. My main task here was to divide these cases and apply different algorithms to take these difference into consideration.

## Solution

My first approach was to divide cases into whether a certain pixel is a null value or not. However, I was (and still am) not sure if the type of value is `NULL`, when I select a pixel that does not exsists.

The actual solution to this was quite simple. When adding the values to the caculation, I added a condition to check whether the pixel is with in the ranges of the size of the image. In other words, a term is only added to the caculation if the row value was between 0 and the height and the column value is between 0 and the width.

``` C
if (i >= 0 && i < height && j >= 0 && j < width)
{
    avgRed += tempImg[i][j].rgbtRed;
    avgGreen += tempImg[i][j].rgbtGreen;
    avgBlue += tempImg[i][j].rgbtBlue;
    count++;
}
```

In the case above i is the row value and j the height value.

---

## 2. Understanding Rounding of Floats in C

Problem same as above

## Obstacle
When I thought I was done with the codes, the main problem I encountered after checking the answer was that my answers were just a few numbers different from the answer. This had to be a problem with rounding of the numbers. When writing the code initially, I did take this into account, and I thought I set the type to floats where needed.

## Solution
While solving this, I came accross the tip to maintain all the datatype of the relative numbers to float all the way to the end of the cacluation and transfer the value to an int at the very end, just before assigning the changed value. The following code was written with the solution above.

```C
float avgRed;
float avgGreen;
float avgBlue;
int count;

// Loop through each pixel
for (int h = 0; h < height; h++)
{
    for (int w = 0; w < width; w++)
    {
        // Reset all calculation variables for each pixel
        avgRed = 0;
        avgGreen = 0;
        avgBlue = 0;
        count = 0;

        // Loop through surrounding pixels
        for (int i = h - 1; i < h + 2; i++)
        {
            for (int j = w - 1; j < w + 2; j++)
            {
                // Do not add into calculation if surrounding pixel is outside of image
                if (i >= 0 && i < height && j >= 0 && j < width)
                {
                    avgRed += tempImg[i][j].rgbtRed;
                    avgGreen += tempImg[i][j].rgbtGreen;
                    avgBlue += tempImg[i][j].rgbtBlue;
                    count++;
                }
            }
        }

        // Get rounded average
        avgRed = round(avgRed / count);
        avgGreen = round(avgGreen / count);
        avgBlue = round(avgBlue / count);

        // Assign average value to image
        image[h][w].rgbtRed = cap(avgRed);
        image[h][w].rgbtGreen = cap(avgGreen);
        image[h][w].rgbtBlue = cap(avgBlue);
    }
}
```

Initially, the avg value for each color was declared as int, because I thought that there isn't going to be a float number assign to a color value. And therefore, in the end, where the division for average happened, I tried to do only the math in floats and directly rounded the value. This did show a very similar outcome to the answer, considering that the values were only a few numbers different. However, when corrected to the above code, the answer was checked correct.

This should mean that when dealing with numbers that might be in calculation in requirement of floats, all the variables are better declared as float, instead of rounding them whenever needed.