# Blobs, Layers, and Nets: anatomy of a Caffe model

Following: http://caffe.berkeleyvision.org/tutorial/net_layer_blob.html

Caffe defines a net layer-by-layer in its own model schema. As data and
derivatives move through the network (forward and back) Caffe stores and
manipulates the information as _blobs_ - a standard array and unified memory
interface. The _layer_ then is the foundation of model and computation, and a
_net_ is a collection of connected layers.

To convert an image, etc. into a blob, see, for example:
https://github.com/BVLC/caffe/blob/master/tools/convert_imageset.cpp


## Blob storage and communication

A blob is an N-dimensional array stored in a C-contiguous fashion. Blobs
provide a unified memory interface for data - batches of images, model
parameters, derivatives, etc. They also handle mixed CPU/GPU operation by
synchronizing from the CPU host to the GPU as required.

Conventional dimensions are number (N) x channel (K) x height (H) x width (W).
Memory is row-major in layout, so the last dimension changes the fastest.

    (n, k, h, w) = ((n * K + k) * H + h) + W * w

Here, Number/N is the batch size and Channel/K is the feature dimension (== 3
for RGB images).

For non-image applications there are 2D blobs (N, D) we may use with the
`InnerProductLayer`.

Parameter blob dimensions vary according to the type and settings of the layer.
A convolution layer with 96 filters of 11x11 spatial extent and 4 inputs is
96 x 3 x 11 x 11. An inner-product layer with 1000 output channels and 1024
inputs is 1000 x 1024.

Custom data types my require custom input preparation tools or data layers, but
the modularilty of layers makes the output simple to use.

### Implementation Details


