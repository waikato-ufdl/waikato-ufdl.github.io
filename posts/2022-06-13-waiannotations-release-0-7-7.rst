.. title: wai.annotations release 0.7.7
.. slug: 2022-06-13-waiannotations-release-0-7-7
.. date: 2022-06-13 16:42:00 UTC+12:00
.. tags: release
.. category: data
.. link: 
.. description: 
.. type: text


A new release of `wai.annotations <https://github.com/waikato-ufdl/wai-annotations>`__ is out now: `0.7.7 <https://github.com/waikato-ufdl/wai-annotations/releases/tag/v0.7.7>`__

Here is an overview of all the changes since the 0.7.6 release:

* wai.annotations.bluechannel 1.0.2

  * `FromBlueChannel` class now outputs logging message if setting of new annotation indices fails, as error
    occurs before the `wai.annotations - Sourced ...` logging message, making it possible to track the image
    causing the problem.

* wai.annotations.coco 1.0.2

  * method `located_object_to_annotation` (module: `wai.annotations.coco.util._located_object_to_annotation`) skips
    invalid polygons now rather than stopping the conversion

* wai.annotations.core 0.1.7

  * Added `discard-invalid-images` ISP for removing corrupt images or annotations with no image attached.
  * Added `batch-split` sub-command for splitting individual batches of annotations into subsets like train/test/val.
    Supports grouping of files within batches (eg multiple images of the same object).
  * Added `filter-metadata` ISP for filtering object detection.
  * Restricted maximum characters per line in help output to 100 to avoid long help strings to become unreadable.
  * The `polygon-discarder` now annotations that either have no polygon or invalid polygons.
  * Added descriptions to the help screens of the main commands.
  * The `ImageSegmentationAnnotation` class now outputs the unique values in its exception when there are
    more unique values than labels
  * The `Data` class (module: `wai.annotations.core.domain`) now outputs a warning message if a file cannot
    be read; also added `LoggingEnabled` mixin.

* wai.annotations.grayscale 1.0.1

  * `FromGrayscale` class now uses `numpy.frombuffer` instead of deprecated `numpy.fromstring` method.
  * `FromGrayscale` class now outputs logging message if setting of new annotation indices fails, as error
    occurs before the `wai.annotations - Sourced ...` logging message, making it possible to track the image
    causing the problem.

* wai.annotations.imgaug 1.0.5

  * Added `sub-images` plugin for extracting regions (including their annotations) based on
    one or more bounding box definitions from the images coming through and only forwarding
    these sub-images

* wai.annotations.imgstats 1.0.3

  * `label-dist-od` now has `--label-key` option to specify the meta-data key that stores the label
  * `label-dist-XY` now sort the labels in alphabetically order before outputting the statistics
  * added `area-histogram-is` and `area-histogram-od`

* wai.annotations.imgvis 1.0.3

  * added `combine-annotations-od` for combining overlapping annotations between images into single annotation
  * `add-annotation-overlay-is/od` now use `narg='+'` for `--labels` and `--colors` options instead of comma-separated single argument

* wai.annotations.indexedpng 1.0.1

  * `FromIndexedPNG` class now outputs logging message if setting of new annotation indices fails, as error
    occurs before the `wai.annotations - Sourced ...` logging message, making it possible to track the image
    causing the problem.

* wai.anntations.redis.predictions 1.0.2

  * `redis-predict-is` now supports `grayscale` predictions as well

* wai.annotations.roi 1.0.1

  * `FromROI.convert_roi_object` now also adds `label`, `score`, `minrect_w` and `minrect_h` to the
    meta-data of a `LocatedObject` instance.

* wai.annotations.video 1.0.1

  * Fixed error message of DropFrames/SkipSimilarFrames in case data of wrong domain is coming through
  * added: filter-frames-by-label-od
