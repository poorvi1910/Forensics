## LITCTF: Kuwahara
Reference: https://liminova.github.io/blog/litctf-2024-kuwahara

Brief explanantion of the writeup:

The challenge provided a noisy image and an encoded image. The goal was to extract the flag from the encoded image.The encoded image was created by applying the Kuwahara filter to the noisy image, which involved hiding the message using a base-3 encoding. This filter works by dividing the image into regions, calculating the average color and standard deviation in each region, and then replacing each pixel with the color from the region with the lowest variation.
By determining which region's average color was used in each part of the encoded image, they could reverse the base-3 encoding to extract the original message.
The solver initially guessed the window size (the size of the regions) incorrectly, leading to an unsuccessful decoding attempt. And even after correcting the window size, the decoded message had errors. These were due to small inaccuracies in the color values, likely caused by floating-point errors in the calculations.
To overcome these issues, the solver modified their approach by considering all possible regions (not just the one with the lowest variation) when trying to decode each character.
When the decoded values were slightly off (by a value of 1), the solver manually adjusted them to the correct base-3 values.

This was a very scripting intensive challenge and i got to know about the filter, reversing procedure and error correction. 

## CTFzone: losing information
