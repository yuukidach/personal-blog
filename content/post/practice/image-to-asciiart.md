---
title: Convert Image to ASCII Art
date: 2020-02-19
categories: [Practice]
tag: [Image Processing, C++]
---

## Background

### Image Processing Library

To process a image, the easiest way is using a open source library.

Since this project is written for others. They are not familiar with the image processing and it's to hard for them to install OpenCV in their computer. So, I tried to find a light-weight image processing processing library called [CImg](https://cimg.eu/).

To use this repo, we just need to put the header file `CImg.h` into our own project folder and include it in the program.

**NOTE: The `CImg` library can only deal with bmp file. In my project we need to deal with a jpg image, so we need to download ImageMagick in our computer to convert the jpg image into bmp image**

### Conversion Principle

Because we will use ASCII code to represent the image, and the ASCII code only has one color, we only need to process a gray image. So the conversion steps become very simple:

1. If the image is not gray, convert it to gray image
2. Crop the image to the desired size (usually smaller than its original size)
3. Define a string `S` which contains different ASCII code.(And its length `l` is better to be short)
4. Convert the pixel value to a number `res` not bigger than `l`
5. Subsitude the pixels with `S[res]`

## Code

``` c++
#include <iostream>
#include <fstream>

#undef cimg_display
#define cimg_display 0              // no need to show image
#include "CImg.h"

using namespace cimg_library;
using namespace std;

const char *ASCII_LIST = "$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/|()1{}[]?-_+~<>i!lI;:,\"^`'. ";

CImg<> rgb2gray(CImg<> color_img)
{
    // create a gray image
    CImg<> gray_img(color_img.width(), color_img.height(), 1, 1, 0);

    // for all pixels x,y in image
    cimg_forXY(color_img, x, y) {
        // separation of channels
        int R = (int) color_img(x, y, 0, 0);
        int G = (int) color_img(x, y, 0, 1);
        int B = (int) color_img(x, y, 0, 2);

        // calculate gray value
        // (x, y) -> val_a,   (x, y) -> a, b, c
        int gray_val = (int) (0.299 * R + 0.587 * G + 0.114 * B);
        gray_img(x, y, 0, 0) = gray_val;
    }
    gray_img.normalize(0, 255);
    return gray_img;
}


/*
 * The input must be gray image. If not, use 'rgb2gray'
 * to do a shift.
 *
 * gray_img: gray image
 * file_name: output file name
 * w: weight
 * h: height
 */
void print_gray2ascii(CImg<> gray_img, const char* file_name, int w, int h)
{
    // output to out.txt
    ofstream out(file_name);

    gray_img.resize(w, h);

    cimg_forY(gray_img,y) {
        cimg_forX(gray_img,x) {
            int val = gray_img(x, y, 0, 0) / sizeof(ASCII_LIST);
            out << ASCII_LIST[val];
        }
        out << endl;
    }

    out.close();
}


int main(void)
{
    cimg::imagemagick_path("D:\\\\\"Program Files\"\\ImageMagick-7.0.9-Q16\\magick.exe");

    // Init images
    CImg<> img("test.jpg");

    img = rgb2gray(img);

    const char *file_name = "out.txt";
    print_gray2ascii(img, file_name, 200, 80);

    return 0;
}
```
