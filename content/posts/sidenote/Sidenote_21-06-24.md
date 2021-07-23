---
title: "Sidenote_21.06.24"
categories: ["Sidenote"]
date: 2021-06-25T23:22:12+09:00
draft: false
tags: ["CS50", "C", "File", "buffer", "sprintf", "Byte"]
---

references:

[File](https://modoocode.com/68)

[sprintf](https://jhnyang.tistory.com/314)

# Task and Solution Code

From a damaged (or deleted) card data, recover original jpeg images. [[details]](https://cs50.harvard.edu/x/2021/psets/4/recover/#:~:text=submit50%20cs50/problems/2021/x/recover)

## My Solution

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>

typedef uint8_t  BYTE;

int main(int argc, char *argv[])
{
    // Check if parameter validity
    if (argc != 2)
    {
        printf("Usage: ./recover image\n");
        return 1;
    }

    // Open input file
    char *infile = argv[1];
    FILE *inptr = fopen(infile, "r");
    if (inptr == NULL)
    {
        fprintf(stderr, "Could not open %s. \n", infile);
        return 1;
    }

    BYTE buffer[512];
    int filecount = 0;
    char filename[8];
    FILE *outptr;

    // Loop while block in the file is readable
    while (fread(&buffer, 512, 1, inptr))
    {
        // Write new jpeg file
        if (buffer[0] == 0xff && buffer[1] == 0xd8 
            && buffer[2] == 0xff && (buffer[3] & 0xf0) == 0xe0)
        {
            // If first jpeg file just open new file
            if (filecount == 0)
            {
                sprintf(filename, "%03i.jpg", filecount);
                filecount++;
                outptr = fopen(filename, "w");
                fwrite(&buffer, 512, 1, outptr);
            }
            // If jpeg file is already open, close current, open new
            else
            {
                fclose(outptr);
                sprintf(filename, "%03i.jpg", filecount);
                filecount++;
                outptr = fopen(filename, "w");
                fwrite(&buffer, 512, 1, outptr);
            }
        }
        else
        {
            // If jpeg file is open and no new jpeg file starts, keep writing
            if (outptr != NULL)
            {
                fwrite(&buffer, 512, 1, outptr);
            }
        }
    }

    // Close all files
    fclose(outptr);
    fclose(inptr);

    return 0;
}
```
<br>

# Sidenote 1 - Read and Write Files
```c
while (fread(&buffer, 512, 1, inptr))
    { 
        ...
    }
```
The fread function reads the file reads the designated size of data (in this case `512` Bytes), designated number of times (in this case `1`) from an open file (in this case `inptr`) and stores it in the `buffer`. After the read, the function returns the number of items read. If the file reaches the end of file, it returns an error. Put in the while loop, the loop reads the input file block by block until the file ends.

```c
fwrite(&buffer, 512, 1, outptr);
```
In practically the same mechanism, the fwrite function writes the content of the buffer into the output file.

# Sidenote 2 - sprintf
```c
sprintf(filename, "%03i.jpg", filecount);
```
`sprintf` function prints the given content into a string.

# Sidenote 3 - Creating the type "BYTE"
In order to define a type called "BYTE" that can be used to store a byte of data, the below method can be used.
```c
#include <stdint.h>

typedef uint8_t  BYTE;
```