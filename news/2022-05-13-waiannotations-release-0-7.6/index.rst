.. title: wai.annotations release 0.7.6
.. slug: 2022-05-13-waiannotations-release-0-7.6
.. date: 2022-05-13 13:15:00 UTC+12:00
.. tags: release
.. category: data
.. link: 
.. description: 
.. type: text


A new release of `wai.annotations <https://github.com/waikato-ufdl/wai-annotations>`__ is out now: `0.7.6 <https://github.com/waikato-ufdl/wai-annotations/releases/tag/v0.7.6>`__

Since the 0.7.3 release, the following changes occurred:

* Added `wai.annotations.imgaug <https://github.com/waikato-ufdl/wai-annotations-imgaug>`__ for performing image augmentations on the image stream (crop, flip, blur, grayscale, contrast, rotate, scale, )
* Added `wai.annotations.imgstats <https://github.com/waikato-ufdl/wai-annotations-imgstats>`__ for generating label distributions
* Added `wai.annotations.imgvis <https://github.com/waikato-ufdl/wai-annotations-imgvis>`__ for adding annotation overlays, viewing images, combining annotations into single overlay image
* Added `wai.annotations.opex <https://github.com/waikato-ufdl/wai-annotations-opex>`__ for reading/writing the `OPEX <https://github.com/WaikatoLink2020/objdet-predictions-exchange-format>`__ object detection JSON format
* Added `wai.annotations.redis.predictions <https://github.com/waikato-ufdl/wai-annotations-redis-predictions>`__ for making predictions via a Redis backend (e.g., image classification, object detection, image segmentation)
* Added `wai.annotations.video <https://github.com/waikato-ufdl/wai-annotations-video>`__ for reading frames from video files/webcams, dropping frames, skipping similar frames, writing video files
* Added `wai.annotations.yolo <https://github.com/waikato-ufdl/wai-annotations-yolo>`__ for reading/writing [YOLO txt files](https://github.com/waikato-ufdl/wai-annotations-yolo/issues/1)
* Upgraded `wai.annotations.core <https://github.com/waikato-ufdl/wai-annotations-core>`__ version, adding ISP for discarding polygons, I/O support for images (with empty annotations)
* Upgraded `wai.annotations.tf <https://github.com/waikato-ufdl/wai-annotations-tf>`__ version to support tensorflow 2.7.x

On top of that, the documentation available through `ufdl.cms.waikato.ac.nz/wai-annotations-manual <https://ufdl.cms.waikato.ac.nz/wai-annotations-manual/>`__
has been overhauled and the example section expanded to include examples for some of the new modules/plugins.

From now on, releases will be made available as `Docker images <https://ufdl.cms.waikato.ac.nz/wai-annotations-manual/docker/>`__
under the following :

`hub.docker.com/repository/docker/waikatoufdl/wai.annotations <https://hub.docker.com/repository/docker/waikatoufdl/wai.annotations>`__

Finally, a working example of using a pretrained Yolov5 network to annotated (and visualize) frames extracted
from a dashcam video can be found in the `example section <https://ufdl.cms.waikato.ac.nz/wai-annotations-manual/examples_redis_predictions/>`__.

.. image:: ../images/dashcam_annotated.png
