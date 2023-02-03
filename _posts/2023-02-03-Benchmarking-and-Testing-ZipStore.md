---
title: "Benchmarking and Testing ZipStore"
Layout: post
categories: outreachy
---

# [Benchmarking](https://github.com/caviere/script/blob/master/sample.py)

Introducing Zarr, a library for efficient array storage and computation and its Python implementation. In this blog, I will explore how to create Zarr arrays, check their integrity and measure the read and write times for different stores.
The benchmark class initializes a set of arrays with random integers. The run method takes the store to be benchmarked as an input and performs the write and read operations. The write operation stores the original array in the specified store and the read operation retrieves the array from the store, computes its checksum and compares it to the original array's checksum.
The stores being benchmarked are the following:

* ZipStore, where the data is compressed and stored in a zip file.
* Fsspec store, where the data is stored in an in-memory file system.
* Directory store, where the data is stored as individual files in a directory.

The results of the benchmark are displayed as bar charts using Plotly. The bar chart shows the time taken to write to and read from the stores. The results demonstrate that the write time for the in-memory store is faster than that of the zip file and the directory store. The read time for the in-memory store and the zip file is similarly fast but the directory store takes longer to read from.
In conclusion, the benchmark shows that Zarr arrays can be stored and retrieved efficiently from different stores. The choice of store depends on the use case and the trade-off between write speed and read speed. For example, the in-memory store is fast for read and write operations but the data is lost when the program terminates. The zip file and the directory store are slower for write operations but the data is persistent and can be accessed across sessions.

![Screenshot from 2023-02-01 11-17-23](https://user-images.githubusercontent.com/110189834/216596002-9c09b787-c237-497c-be7b-7def2776991e.png)

# [Testing ZipStore Functionality](https://github.com/caviere/testing_zipstore/blob/main/real%20%20world%20data/main.py)

The code tests the Zipstore in the following libraries: zarr, xarray, netcdf4, gdal, h5py, and fsspec.

I started by testing the Zipstore with the zarr library. As seen in the output, the code is able to open the Zipstore file in read mode with the zarr library with no issues. This shows that the zarr library supports reading and writing data in a Zipstore.

Next, I tested the Zipstore with the xarray library. The code is able to open the Zipstore file with the xarray library and read its data. This means that xarray can be used to read and write data in a Zipstore.

The netcdf4 library does not have direct support for reading and writing data in a Zipstore. As seen in the output, the netcdf4 library is unable to open the Zipstore file with the provided code.

The gdal library fails to open the dataset.

The h5py library is unable to open the Zipstore file as it is not a valid HDF5 file. This means that h5py cannot be used to read or write data in a Zipstore.

Finally, I tested the Zipstore with the fsspec library. The code is unable to open the Zipstore file as the ZipStore object does not have the required attribute.

In conclusion, the results show that the zarr and xarray libraries have full support for reading and writing data in a Zipstore. The gdal, the netcdf4 and h5py libraries do not have full support for Zipstores. The fsspec library requires further investigation to determine its support for Zipstores.

# [Mnist Dataset](https://github.com/caviere/testing_zipstore/blob/main/py/example.py)

This script is a simple Python program that loads the MNIST dataset, which consists of 60,000 28x28 grayscale images of handwritten digits along with their corresponding labels and saves it in a compressed format using the Zarr library. The first two lines of code handle logging for TensorFlow which is set to minimum. The script then uses TensorFlow's keras.datasets module to load the MNIST data into numpy arrays and asserts the shape of the arrays to verify that the data was loaded correctly. Finally, the script creates a ZipStore using the Zarr library and saves the training images to the store by creating a Zarr array and assigning the images to it. The compressed dataset is saved to the disk and then the store is closed.

