.. title: wai.annotations modularized
.. slug: 2021-05-28-waiannotations-modularized
.. date: 2021-05-28 16:05:00 UTC+12:00
.. tags: release
.. category: data
.. link: 
.. description: 
.. type: text


A new release of `wai.annotations <https://github.com/waikato-ufdl/wai-annotations>`__ is out: `0.7.1 <https://github.com/waikato-ufdl/wai-annotations/releases/tag/v0.7.1>`__

The 0.7.x series saw a modularization happening behind the scenes, with each file format now being a separate repository and Python package. 
This makes it much easier to only include the relevant dependencies in ones virtual environment or docker image, keeping things as small as possible.
