.. title: ADAMS snapshots now publicly available
.. slug: 2020-07-24-adams-snapshots
.. date: 2020-07-24 09:41:00 UTC+12:00
.. tags: release
.. category: library
.. link: 
.. description: 
.. type: text


The newly available `ufdl-frontend-adams <https://github.com/waikato-ufdl/ufdl-frontend-adams>`__ modules for
`ADAMS <https://adams.cms.waikato.ac.nz/>`__ now have public builds available for download:

`adams.cms.waikato.ac.nz/snapshots/ufdl/ <https://adams.cms.waikato.ac.nz/snapshots/ufdl/>`__

As of now, the following workflows can be used for managing a UFDL server instance and datasets:

* *adams-ufdl-core-manage_backend.flow* - manages users, teams, projects, licenses
* *adams-ufdl-image-manage_image_classification_datasets.flow* - for image classifications datasets
* *adams-ufdl-image-manage_object_detection_datasets.flow* - for object detection datasets
* *adams-ufdl-speech-manage_speech_datasets.flow* - for speech datasets

**NB:** In order to utilize these flows, you need to have an instance of the 
`ufdl-backend <https://github.com/waikato-ufdl/ufdl-backend>`__ running, of course.