.. title: wai.annotations release 0.4.0
.. slug: 2020-07-02-waiannotations-release-0-4-0
.. date: 2020-07-02 11:57:00 UTC+12:00
.. tags: release
.. category: data
.. link: 
.. description: 
.. type: text


A new release of `wai.annotations <https://github.com/waikato-ufdl/wai-annotations>`__ is out now: `0.4.0 <https://github.com/waikato-ufdl/wai-annotations/releases/tag/v0.4.0>`__

This release represents a major refactoring, introducing domains other than object-detection in images. 
Another notable change is being able to add filters into the conversion pipeline. This will, e.g., allow the
augmentation of datasets while converting them. In terms of object detection datasets, these augmentations can be 
rotation, cropping, brightening/darkening, etc. Though a lot of frameworks already support basic augmentation
transformations, trying to automatically augment bounding boxes rather than object shapes can lead to strange
and undesirable artifacts (e.g., overly large boxes, overlapping annotations).
