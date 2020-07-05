.. title: MMDetection Docker image using CUDA 10
.. slug: 2020-06-17-mmdetection-docker-cuda10
.. date: 2020-06-17 10:09:00 UTC+12:00
.. tags: release
.. category: docker
.. link: 
.. description: 
.. type: text


The previously release Docker images for MMDetection only worked with PyTorch 1.3 on CUDA 10.1, which only worked on NVIDIA 2080 Ti cards, but not 1080 Ti ones. Today, we released a new Docker image which is based on PyTorch 1.2 and CUDA 10.0, which allows 1080 Ti cards to be used again:

`github.com/waikato-datamining/mmdetection/tree/master/2020-03-01_cuda10 <https://github.com/waikato-datamining/mmdetection/tree/master/2020-03-01_cuda10>`__
