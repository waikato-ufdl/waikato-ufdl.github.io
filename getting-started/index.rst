.. title: Getting started
.. slug: getting-started
.. date: 2020-09-15 15:23:32 UTC+12:00
.. tags:
.. category:
.. link:
.. description:
.. type: text

.. contents::

At the time of writing, the overall system is not yet production-ready. However, if you want to check out
what the system is about, you can follow these instructions to set up a local development system that runs
on your own infrastructure. Of course, for most deep learning training or prediction tasks you will
require a NVIDIA GPU.

The server backend and worker nodes are expected to run Linux (tested with Ubuntu 18.04). The frontend,
e.g., when using ADAMS can be Linux, Windows or Mac.


Set up the backend
==================

Clone the following repositories (within the same directory):

* `ufdl-backend <Backend_>`__
* `ufdl-json-messages <JsonMessages_>`__

.. code:: bash

   git clone https://github.com/waikato-ufdl/ufdl-backend
   git clone https://github.com/waikato-ufdl/ufdl-json-messages

Change into the directory of the cloned *ufdl-backend* repository and run the following script to set up
the virtual environment for the server (**CAUTION:** it will delete any previously stored data and the database):

.. code:: bash

   ./dev_init.sh

Once this has completed, you can start up the REST API on ``127.0.0.1`` as follows:

.. code:: bash

   ./dev_start.sh

Use ``0.0.0.0:8000`` as argument if you want to make the server available to the outside world on port 8000.
Ensure that your firewall allows that port to be accessed from the outside.


Initialize the backend
======================

* Download the ZIP file of the `ADAMS frontend <ADAMSFrontend_>`__ and unzip it.
* Start ADAMS with the ``bin/start_gui.sh`` script (Linux/Mac) or ``bin/start_gui.bat`` batch file (Windows).
* Use the *Flow editor* (from the *Tools* menu) to run the ``adams-ufdl-all-basic_setup.flow`` flow for setting up a
  basic environment (users, teams, projects).


Set up a worker node
====================

On the worker node, clone the following repositories (within the same directory):

* `ufdl-json-messages <JsonMessages_>`__
* `ufdl-python-client <PythonClient_>`__
* `ufdl-job-launcher <JobLauncher_>`__

.. code:: bash

   git clone https://github.com/waikato-ufdl/ufdl-json-messages
   git clone https://github.com/waikato-ufdl/ufdl-python-client
   git clone https://github.com/waikato-ufdl/ufdl-job-launcher

Change into the directory of the cloned *ufdl-job-launcher* repository and run the following script to set up
the virtual environment:

.. code:: bash

   ./dev_init.sh

In the ``examples`` directory, you can copy the ``job-launcher-example.conf`` configuration to ``job-launcher.conf``
and then update the required parameters (if anything, should be only the ``url``).

Once this suits your system, you can start the job-launcher like this (from within the ``ufdl-job-launcher`` directory):

.. code:: bash

   ./venv.dev/bin/ufdl-joblauncher -C examples/job-launcher.conf -C


Use the system
==============

The following ADAMS flows are available to manage your datasets and run jobs (simply execute them with the *Flow editor*):

* ``adams-ufdl-core-manage_backend.flow`` - for managing the backend, starting jobs, etc.
* ``adams-ufdl-image-manage_image_classification_datasets.flow`` - manages image classification datasets
* ``adams-ufdl-image-manage_objected_detection_datasets.flow`` - manages object detection datasets
* ``adams-ufdl-speech-manage_speech_datasets.flow`` - manages speech datasets


.. _Backend: https://github.com/waikato-ufdl/ufdl-backend
.. _JsonMessages: https://github.com/waikato-ufdl/ufdl-json-messages
.. _PythonClient: https://github.com/waikato-ufdl/ufdl-python-client
.. _JavaClient: https://github.com/waikato-ufdl/ufdl-java-client
.. _ADAMSFrontend: https://adams.cms.waikato.ac.nz/snapshots/ufdl/
.. _JobLauncher: https://github.com/waikato-ufdl/ufdl-job-launcher
