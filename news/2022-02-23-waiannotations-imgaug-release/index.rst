.. title: wai.annotations.imgaug release
.. slug: 2022-02-23-waiannotations-imgaug-release
.. date: 2022-02-23 16:40:00 UTC+13:00
.. tags: release
.. category: data
.. link: 
.. description: 
.. type: text

The newly released `wai.annotations.imgaug <https://github.com/waikato-ufdl/wai-annotations-imgaug>`__ module for the
`wai.annotations <https://github.com/waikato-ufdl/wai-annotations>`__ library offers some basic image augmentation
techniques (crop, flip, gaussian-blur, hsl-grayscale, linear-contrast, rotate) which can be applied to the stream
of images that are being processed in the pipeline. Of course, any annotations get processed accordingly.
Under the hood, the excellent `imgaug <https://github.com/aleju/imgaug>`__ library is utilized.

At this stage, the module supports the following domains:

* Image Classification Domain
* Image Object-Detection Domain
