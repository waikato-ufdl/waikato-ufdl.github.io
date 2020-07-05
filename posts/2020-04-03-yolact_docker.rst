.. title: YOLACT/YOLACT++ Docker images available
.. slug: 2020-04-03-yolact-docker
.. date: 2020-04-03 11:38:00 UTC+13:00
.. tags: release
.. category: docker
.. link: 
.. description: 
.. type: text


The first Docker images are available for the `YOLACT <https://github.com/dbolya/yolact/>`__ (You Only Look At CoefficienTs) image segmentation model, using the 1.2 release:

`github.com/waikato-datamining/yolact <https://github.com/waikato-datamining/yolact>`__

YOLACT/YOLACT++, like MMDetection, is based on `PyTorch <https://pytorch.org/>`__. In contrast to MMDetection it is not a divers framework, but just a single model, which can make use of various base models (e.g., ResNet50). Similar to Mask R-CNN, it generates a mask with probabilities for each identified object, which requires post-processing to determine a `polygon <https://scikit-image.org/docs/0.17.x/auto_examples/edges/plot_contours.html>`__ or `minimal rectangle <https://docs.opencv.org/2.4/modules/imgproc/doc/structural_analysis_and_shape_descriptors.html?highlight=minarearect#minarearect>`__. However, it is much faster than the Mask R-CNN implementation that TensorFlow's Object Detection API offers (roughly 3x faster on 1000x300 images).