---
title: "Script Testing ZipStore"
Layout: post
categories: outreachy
---

# [Script](https://github.com/caviere/script/blob/master/sample.py)
Introducing Zarr, a library for efficient array storage and computation, and its Python implementation. In this blog, we will explore how to create Zarr arrays, check their integrity and measure the read and write times for different stores.
The benchmark class initializes a set of arrays with random integers. The run method takes the store to be benchmarked as an input, and performs the write and read operations. The write operation stores the original array in the specified store and the read operation retrieves the array from the store, computes its checksum, and compares it to the original array's checksum.
The stores being benchmarked are the following:
* ZipStore, where the data is compressed and stored in a zip file
* Fsspec store, where the data is stored in an in-memory file system
* Directory store, where the data is stored as individual files in a directory
The results of the benchmark are displayed as bar charts using Plotly Express. The bar chart shows the time taken to write to and read from the stores. The results demonstrate that the write time for the in-memory store is faster than that of the zip file and the directory store. The read time for the in-memory store and the zip file is similarly fast, but the directory store takes longer to read from.
In conclusion, the benchmark shows that Zarr arrays can be stored and retrieved efficiently from different stores. The choice of store depends on the use case, and the trade-off between write speed and read speed. For example, the in-memory store is fast for read and write operations, but the data is lost when the program terminates. The zip file and the directory store are slower for write operations, but the data is persistent and can be accessed across sessions.

![Screenshot from 2023-02-01 11-17-23](https://user-images.githubusercontent.com/110189834/216596002-9c09b787-c237-497c-be7b-7def2776991e.png)
