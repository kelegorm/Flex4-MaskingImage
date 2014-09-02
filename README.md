Masking Image
==================

This Flex 4.6 project is demonstrating how to mask one BitmapData with another BitmapData.

## Main explanation

##### What we have 

We have two images:

1. Original image with alpha chanel
2. Mask image (White image with alpha chanel)

We need to apply mask to original image.

##### How we can do this

To get result in new Bitmap we do:

    var originalBitmap:BitmapData = /* smth */;
    var maskBitmap:BitmapData = /* smth */;

    var resultBitmap:BitmapData = new BitmapData(size, size, true, 0x00000000);
    resultBitmap.copyPixels(originalBitmap, rect, point, maskBitmap, point, true);
    
If we want to change originalBitmap we do this after previous step:

    originalBitmap.copyChannel(resultBitmap, rect, point, BitmapDataChannel.ALPHA, BitmapDataChannel.ALPHA);


##### What is main moment

Main moment is we should to do `copyPixels()` to EMPTY and TRANSPARENT background. Why we can do this to original image? I'll tell you.

Our Exapmle:

    targetBitmap.copyPixels(originalBitmap, rect, point, maskBitmap, point, true);

Let's split `copyPixels()` work to few steps:
1. It takes a originalBitmap and maskBitmap and generate a new image with merged alpha chanels. I call it 'mergeResult'.
2. But next step `copyPixels()` just draws a mergeResult over targetBitmap with NORMAL blend mode. It doesn't replace a target pixels with mergeResult pixels.

So, if targetBitmap has a image already, we have a result of drawing merged image to it. If targedBitmap is a originalBitmap we don't see any changes after `copyPixels()`.

##### What doesn't work

I tryed a different ways to do this. We can't to do this with `draw()` function with any BlensMode parameter, I tryed all of them. I tryed a way with ERASE blend mode, it doesn't work becouse it make background black and not transparent.

Forgot about all another ways, this works fine.
