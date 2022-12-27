---
title: "Everybody struggles"
Layout: post
categories: outreachy
---

# A brief overview of my tasks
It has been three weeks since I started my internship at zarr and the two tasks I have been working on are going through zarr’s codebase, documentation, specification and [creating a zipstore in zarr-python and testing the support across various zarr implementations](https://github.com/caviere/testing_zipstore). With the help of my mentors [Josh](https://github.com/joshmoore) and [Sanket](https://github.com/MSanKeys963), we identified implementations that I can test out. 

One of the implementations is Jzarr. [Jzarr](https://jzarr.readthedocs.io/en/latest/) is a Java library providing an implementation of chunked, compressed, N-dimensional arrays that is close to the python [zarr package](https://zarr.readthedocs.io/en/stable/index.html). Being a java-novice, it's been a real eye-opener. I was successful in installing the necessary tools required. Added the Jzarr maven dependency so that I can make use of the library but integration has yet to be successful. My mentors and I are looking into the [issue](https://github.com/zarr-developers/outreachy/issues/4) and it is safe to say I'm not ready for the big leagues just yet but I'm definitely getting a taste of the java life.

Alongside Jzarr, I have tested out other four implementations namely: 
* [H5py](https://www.h5py.org/) 
* [Fsspec](https://filesystem-spec.readthedocs.io/en/latest/)
* [Gdal using the zarr driver](https://gdal.org/drivers/raster/zarr.html#raster-zarr)
* [Netcdf nczarr implementation](https://docs.unidata.ucar.edu/nug/current/nczarr_head.html#nczarr_zip) 

These implementations efficiently support zarr zip stores as well as offer a seamless interoperbility. I have also tested out different shapes and chunks. It is safe to say the optimal chunks should be 10000 laid out in a 100 by 100 grid.

# Tips
I’d say what has made the internship easier is breaking a task into doable bits, having a routine, being disciplined and most importantly, keeping tabs with my mentors. Their guidance on how to handle tasks has really made it easier for me to tackle them which I’m grateful for. 
