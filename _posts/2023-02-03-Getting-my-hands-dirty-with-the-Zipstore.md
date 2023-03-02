---
title: "Getting my hands dirty with the ZipStore"
Layout: post
categories: outreachy
---

# [Testing ZipStore Functionality](https://github.com/caviere/testing_zipstore/blob/main/real%20%20world%20data/main.py)

The code tests the Zipstore in the following libraries: [zarr-python](https://zarr.readthedocs.io/en/stable/index.html), [xarray](https://docs.xarray.dev/en/stable/), [netcdf4](https://docs.unidata.ucar.edu/netcdf-c/current/), [gdal](https://gdal.org/), [h5py](https://docs.h5py.org/en/stable/), and [fsspec](https://filesystem-spec.readthedocs.io/en/latest/).

I started by testing the Zipstore with the zarr library. As seen in the output,  ![Screenshot from 2023-02-05 18-02-38](https://user-images.githubusercontent.com/110189834/216827264-e60c6d21-e891-4056-9b4d-11f598a2f988.png) 

the [code](https://github.com/caviere/testing_zipstore/blob/main/real%20%20world%20data/main.py) is able to open the Zipstore file in read mode with the zarr library with no issues. This shows that the zarr library supports reading and writing data in a Zipstore.

Next, I tested the Zipstore with the xarray library. Xarray is able to open the Zipstore file with the xarray library and read its data. This means that xarray can be used to read and write data in a Zipstore.

The netcdf4 library does not have direct support for reading and writing data in a Zipstore. As seen in the output, the netcdf4 library is unable to open the Zipstore file with the provided code.

The gdal library is able to open and read the dataset.

The h5py library is unable to open the Zipstore file as it is not a valid HDF5 file. This means that h5py cannot be used to read or write data in a Zipstore.

Finally, I tested the Zipstore with the fsspec library. The code is able to open and read the Zipstore file.

In conclusion, the results show that the zarr and xarray libraries have full support for reading and writing data in a Zipstore. The gdal and fsspec libraries are able to open and read data in a Zipstore. The netcdf4 and h5py libraries do not have full support for Zipstores. 

# [MNIST Dataset](https://github.com/caviere/testing_zipstore/blob/main/py/example.py)

This [script](https://github.com/caviere/testing_zipstore/blob/main/py/example.py) is a simple Python program that loads the [MNIST dataset](http://yann.lecun.com/exdb/mnist/), which consists of 60,000 28x28 grayscale images of handwritten digits along with their corresponding labels and saves it in a compressed format using the Zarr library.
```python
(train_images, train_labels), (test_images, test_labels) = tf.keras.datasets.mnist.load_data()
assert train_images.shape == (60000, 28, 28)
assert train_labels.shape == (60000,)

assert test_images.shape == (10000, 28, 28)
assert test_labels.shape == (10000,)

# write to zipstore
store = zarr.ZipStore("data.zip", mode="w")
z = zarr.zeros(train_images.shape, store=store)
z[:] = train_images
store.close()
```

The script then uses TensorFlow's [keras.datasets](https://keras.io/api/datasets/) module to load the MNIST data into numpy arrays and asserts the shape of the arrays to verify that the data was loaded correctly. Finally, the script creates a ZipStore using the Zarr library and saves the training images to the store by creating a Zarr array and assigning the images to it. The compressed dataset is saved to the disk and then the store is closed. 

# [Benchmarking](https://github.com/caviere/testing_zipstore/blob/main/benchmark/main.py)

Introducing Zarr, a format for storing large data in chunked, compressed N-dimensional arrays and its [python implementation](https://github.com/zarr-developers/zarr-python). In this blog, I will explain how I created zarr arrays, checked their integrity and measured the read and write times for different stores.

In the [script](https://github.com/caviere/testing_zipstore/blob/main/benchmark/main.py), the benchmark class initializes a set of arrays with random integers. The run method takes the store to be benchmarked as an input and performs the write and read operations. The write operation stores the original array in the specified store and the read operation retrieves the array from the store, computes its checksum using SHA256 and compares it to the original array's checksum.

The stores being benchmarked are the following:

* [ZipStore](https://zarr.readthedocs.io/en/stable/api/storage.html#zarr.storage.ZipStore), where the data is compressed and stored in a zip file.
* [Fsspec store](https://github.com/fsspec/filesystem_spec), where the data is stored in an in-memory file system.
* [Directory store](https://zarr.readthedocs.io/en/stable/api/storage.html#zarr.storage.DirectoryStore), where the data is stored as individual files in a directory.

The results of the benchmark are displayed as bar charts using [plotly](https://plotly.com/). The bar charts show the time taken to write to and read from the stores. The results demonstrate that the write time for the fsspec is faster than that of the zipstore and the directory store. The read time for the fsspec and the zipstore is similarly fast but the directory store takes longer to read from.

In conclusion, the benchmark shows that zarr arrays can be stored and retrieved efficiently from different stores. The choice of store depends on the use case and the trade-off between write speed and read speed. For example, the fsspec is fast for read and write operations but the data is lost when the program terminates. The zipstore and the directory store are slower for write operations but the data is persistent and can be accessed across sessions. Fsspec array storage limits us to the size of the memory (RAM) available to us while directory storage doesn't have this limitation.

![Screenshot from 2023-02-05 17-58-09](https://user-images.githubusercontent.com/110189834/216827078-710f7f33-19ec-4990-b6ce-4fca83c83486.png)
![Screenshot from 2023-02-05 17-58-18](https://user-images.githubusercontent.com/110189834/216827060-1d6c824b-96d4-4bb1-aace-87a87144d9bc.png)


