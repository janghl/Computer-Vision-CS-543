# Assignment 1: Registering Prokudin-Gorskii color separations of the Russian Empire
# Background
Sergei Mikhailovich Prokudin-Gorskii (1863-1944) was a photographer who, between the years 1909-1915, traveled the Russian empire and took thousands of photos of everything he saw. He used an early color technology that involved recording three exposures of every scene onto a glass plate using a red, green, and blue filter. These are known as color separations. Back then, there was no way to print such photos, and they had to be displayed using a special projector. Prokudin-Gorskii left Russia in 1918. His glass plate negatives survived and were purchased by the Library of Congress in 1948. Today, a digitized version of the Prokudin-Gorskii collection is available online.

# Overview
The goal of this assignment is to learn to work with images by taking the digitized Prokudin-Gorskii glass plate images and automatically producing a color image with as few visual artifacts as possible. In order to do this, you will need to extract the three color channel images, place them on top of each other, and align them so that they form a single RGB color image.

Some details are quite important. You should notice that it matters how you crop the images when you align them -- the separations may not overlap exactly. We have provided an RGB image to check your code on at this location. You should separate this image into three layers (R, G and B), then place each of those layers inside a slightly bigger, all white, layer, at different locations. Now register these three. You can tell whether you have the right answer in two ways: first, you shifted the layers with respect to one another, so you know the right shift to register them; second, if you look at the registered picture, the colors should be pure.

You will need to implement this assignment in Python, and you should familiarize yourself with libraries for scientific computing and image processing including NumPy and PIL.
# Data
A zip archive with six input images for the basic alignment experiments is available here. The high-resolution images for multiscale alignment experiments are available in this archive (the file is over 150MB). Separations: We have made no effort to determine what order the separations are in, and we believe that this changes from file to file. You should try each of the 6 available options and see which gives the most plausible image. This is relatively easy to do (if you get the order wrong for the example image at the top of the page, you'll find that the doors are an implausible color).
# Detailed instructions
Your program should divide the image into three equal parts (channels) and align two of the channels to the third (you should try different orders of aligning the channels and figure out which one works the best). For each input image, you will need to include in your report the colorized output and the (x,y) displacement vectors that were used to align the channels.
Basic alignment. The easiest way to align the parts is to exhaustively search over a window of possible displacements (say [-15,15] pixels independently for the x and y axis), score each one using some image matching metric, and take the displacement with the best score. There is a number of possible metrics that one could use to score how well the images match. The most basic one is the L2 norm of the pixel differences of the two channels, also known as the sum of squared differences (SSD), which in Python is simply sum((image1-image2)**2) for images loaded as NumPy arrays. Note that in our case, the images to be matched do not actually have the same brightness values (they are different color channels), so a cleverer metric might work better. One such possibility is normalized cross-correlation (NCC), which is simply the dot product between the two images normalized to have zero mean and unit norm. Test your basic alignment solution on the first set of six lower resolution images.

# Multiscale alignment 
For the high-resolution glass plate scans provided above, exhaustive search over all possible displacements will become prohibitively expensive. To deal with this case, implement a faster search procedure using an image pyramid. An image pyramid represents the image at multiple scales (usually scaled by a factor of 2) and the processing is done sequentially starting from the coarsest scale (smallest image) and going down the pyramid, updating your estimate as you go. It is very easy to implement by adding recursive calls to your original single-scale implementation.