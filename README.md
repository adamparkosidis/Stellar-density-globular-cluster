# Calculating the stellar density of a Globular Cluster with Python's Numerical Libraries

## Access the Data

[Data](https://drive.google.com/file/d/11UnLbm49pU8bNdL_FicelwxlQ1TvOVM_/view?usp=sharing)

## Introduction

This project is based on an image of a dense star cluster (globular cluster NGC 104, a.k.a. 47 Tuc) obtained by the UVIS detector of the Wide Field Camera 3 (WFC3) on the Hubble Space Telescope. The file containing the image data, `ic2r02050_drz.fits` is in FITS format. The image data we need is in the first (‘SCI’) extension. One of the main goals of this project is to carry out an analysis of the distribution of stars in the cluster, using a variety of sub-packages from `Astropy`, `NumPy` and `Scipy`.

## Code Overview

1. We use the FITS functionality from `Astropy` to open the FITS file. Access the image data in the FITS file and create a plot of it using `Matplotlib`. We   adjust the range of pixel values that are shown by `Matplotlib` in order to show the stars clearly.  The image we initially just create has no sky coordinates along the axes, instead it has pixel coordinates. `Astropy` contains a module `astropy.wcs` which can deal with the World Coordinate System, which allows us to create plots with proper sky-coordinates ([more info](https://docs.astropy.org/en/stable/wcs/index.html). We look through the FITS header and confirm that it contains a WCS section. It is that section that describes how the image pixels are projected on the sky. We create a plot of the globular cluster NGC 104 that shows the stars in the cluster and has axes labeled with sky coordinates. 

2. Next, we use the [astropy.photutils](https://photutils.readthedocs.io/en/stable/) package to detect the stars in the image. The photutils package expects data with the background subtracted and without ```nan``` values, thus we use nump`NumPy` to mask nan-values and determine the median of the image. We subtract the median from the image and after that replace the nan-values with zero. 

3. Now, we use [DAOStarFinder](https://photutils.readthedocs.io/en/stable/api/photutils.detection.DAOStarFinder.html) from photutils to detect the star positions in the image. We use fwhm=3 (the threshold should be small compared to the flux per pixel in a typical star). We plot the detected stars over the image of the NGC 104 globular cluster 

4. We now perform some simple analysis (histograms and function fits) to find the approximate cluster center. We perform our analysis in pixel coordinates. We create create two histograms. One for the x-positions of the detected stars, and one for the y-positions. (We ignore the distortion in the image, and assume the cluster to be circular). We create an array with bin centres (i.e. centre point of each bin),  a function for a one-dimensional Gaussian and we add a term to be able to model the constant background level (due to fake detections and stars in front of the globular cluster). For both the X and Y star position histograms we perform a fit. We fit the Gaussian model we created, to the bin values and bin centers. We provide plots of our histograms with the fitted functions drawn over them.

5. Next, we write a function which calculates the stellar density profile of the cluster: this is the number of stars per square arcsec vs. radius in arcsec from the centre of the cluster. To do so, we first create a histogram of the pixel radii (i.e. radius from the measured cluster center) at which the stars were detected. The UVIS detector pixel size corresponds to 0.04 arcsec per pixel on the actual sky image. We limit the maximum radius considered in order to not include the edges of the image, which distort the measurement (we recommend limiting the radius to < 2000 pixels from the cluster centre). Next we divide the histogram values for each radial bin by the area of that bin (which corresponds to an annulus with inner and outer radii set by the bin edges). We also estimate error bars by taking the square-root of the number of stars in each radial bin and then dividing by the area. We plot your calculated values of stellar density (with error bars) vs. radius using a log-log scale.

6. Finally, we fit the stellar density profile with a [King model](https://ui.adsabs.harvard.edu/abs/1966AJ.....71...64K/abstract) profile, commonly used for globular clusters. We plot our best-fitting model together with the data and errors (also using a log-log scale). Finally, we use your results to calculate and state the ‘core radius’ of the cluster in arcsec.
