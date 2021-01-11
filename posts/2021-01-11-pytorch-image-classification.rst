.. title: PyTorch image classification available
.. slug: 2021-01-11-pytorch-image-classification
.. date: 2021-01-11 16:53:00 UTC+13:00
.. tags: release
.. category: library
.. link: 
.. description: 
.. type: text

Today, a new library for performing image classification has made its debut:

**wai.pytorchimageclass**

The library is based on the PyTorch example code for `imagenet <https://github.com/pytorch/examples/tree/master/imagenet>`__.
For ResNet-based networks, you can finetune pretrained models on your own data rather than
just using the imagenet dataset. In addition, you can make predictions (single and batch/continuous), output information 
on built models, export trained models to `TorchScript <https://pytorch.org/docs/stable/jit.html>`__.

The library is also available via Docker images, one for `GPU-based <https://github.com/waikato-datamining/pytorch/tree/master/image-classification/docker/1.6.0>`__ machines and one for `CPU-only ones <https://github.com/waikato-datamining/pytorch/tree/master/image-classification/docker/1.6.0_cpu>`__. However, the latter one should only be used for inference and not training, as it is simply too slow.

More information on the library and the Docker images is available from Github:

`github.com/waikato-datamining/pytorch/tree/master/image-classification <https://github.com/waikato-datamining/pytorch/tree/master/image-classification>`__
