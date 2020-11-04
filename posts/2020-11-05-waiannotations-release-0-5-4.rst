.. title: wai.annotations release 0.5.4
.. slug: 2020-11-05-waiannotations-release-0-5-4
.. date: 2020-11-05 10:05:00 UTC+13:00
.. tags: release
.. category: data
.. link: 
.. description: 
.. type: text


A new release of `wai.annotations <https://github.com/waikato-ufdl/wai-annotations>`__ is out now: `0.5.4 <https://github.com/waikato-ufdl/wai-annotations/releases/tag/v0.5.4>`__

The introduction of image segmentation required more refactoring behind the scenes, which resulted in the 0.5.x release series.

Highlights since the 0.4.0 release:

* MS COCO format can specify now labels to expect (in case subsets of the dataset do not have all labels present)
* MS COCO format can now sort the determined labels to avoid ordering issues
* MS COCO can write the discovered labels to a text file (comma-separated list)
* Readers/writers no longer assume disk access
* Macro support for simple command-line substitution
* New image classification formats: *subdir* (used by Tensorflow image classification) and *ADAMS* (label of image present in report)
* MS COCO/ROI/VGG object detection formats no longer write negative annotations
* With the *strip-annotations* plugin, all annotations can be stripped during the conversion (e.g., for generating a dataset only consisting of images)
* Image segmentation support: PNG with indexed palette for labels, PNG using blue channel for labels, *layer-segments* format which separates each label into a separate PNG (makes it easier to create subsets of labels)
