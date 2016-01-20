# Blobs, Layers, and Nets: anatomy of a Caffe model

Following: http://caffe.berkeleyvision.org/tutorial/net_layer_blob.html

Caffe defines a net layer-by-layer in its own model schema. As data and
derivatives move through the network (forward and back) Caffe stores and
manipulates the information as _blobs_ - a standard array and unified memory
interface. The _layer_ then is the foundation of model and computation, and a
_net_ is a collection of connected layers.

## Blob storage and communication

A blob is an N-dimensional array stored in a C-contiguous fashion.
