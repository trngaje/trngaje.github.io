---
layout: posts
title:  "how to generate custom font for retroarch rgui"
comments: true
categories : [retroarch]
tags : [retroarch,rgui]
---

Here, I will introduce how to create fonts for retroarch rgui.
Most of them were made by analyzing the retroarch source.
If there are resources that need to be converted, I will review them at any time.
Resources for fonts include bmp, png, font, ttf, and bdf format.
In the traditional retroarch rgui, the bmp font file was converted to a 1 bit color depth and into a file that could be read from c.
I guess the reason why tools for conversion are not readily available is because of their unique structure.

First of all, I will explain the basic font, English font.
It was 5x10 in size at first.

If you analyze the first letter of !...
hexadecimal value

    0x80, 0x10, 0x42, 0x08, 0x20, 0x00, 0x00,

Convert to Binary

    1000 0000
    0001 0000
    0100 0010
    0000 1000
    0010 0000
    0000 0000

Place it in a little endian and cut it into 5 pixels

    00000
    00100
    00100
    00100
    00100
    00100
    00000
    00100
    00000000

The source code in retroarch for this is as follows.

    unsigned font_pixel = x + y * FONT_WIDTH;
    uint8_t rem         = 1 << (font_pixel & 7);
    unsigned offset     = font_pixel >> 3;
    uint8_t col         = (bitmap_bin[FONT_OFFSET(letter) + offset] & rem) ? 0xff : 0;


The font was recreated by IlDucci's request to improve the second Cyrillic language display error.
It was 6x10 in size at first.

![](/images/2022-09-20/bitmap_cyrillic.png)

Each letter is separated by 16 pixels, of which only 6x10 is a meaningful area.

For this purpose, I created a conversion program using the SDL function.


It can be generated simply as follows.

    ./png2c bitmap_cyrillic.png

Please refer to the location below for the code to build.

https://github.com/trngaje/png2c
