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


Set up the PostgreSQL database
==============================

Make sure you have PostgreSQL installed and the server is running, and then add a database for the backend
to use (e.g. by using psql -c COMMAND postgres) (**N.B.** the database name must be *ufdl*):

.. code:: sql

   CREATE DATABASE ufdl;

Create a user for the backend to access the database with (replace the username/password with whatever you like):

.. code:: sql

   CREATE USER username WITH ENCRYPTED PASSWORD 'password';
   GRANT ALL PRIVILEGES ON DATABASE ufdl TO username;

Edit the Host-Based Authentication file for PostgreSQL (usually located at /etc/postgresql/{VERSION}/main/pg_hba.conf)
to allow the user to authenticate with the database. If the backend server will be running on the same machine as the
database, run:

.. code:: bash

   echo "local ufdl username md5" >> /path/to/pg_hba.conf

otherwise:

.. code:: bash

   echo "host ufdl username all md5" >> /path/to/pg_hba.conf

If the backend will not be running on the same machine as the database, the 'listen_addresses' setting in
postgresql.conf also needs to be set to allow the backend to connect (e.g. by setting it to '*').


Set up the PostgreSQL database (Docker)
=======================================

A Docker image which has a preconfigured PostgreSQL database is provided for convenience. To obtain the image, with
the Docker daemon running:

.. code:: bash

   docker login public.aml-repo.cms.waikato.ac.nz:443
   docker pull public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_postgres:latest
   docker tag public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_postgres:latest ufdl_postgres

The username/password for the database user in this image is ufdl/ufdl respectively.

Alternatively, the Dockerfile can be built to change the username/password. Firstly clone the backend repository and
change to the database Docker directory:

.. code:: bash

   git clone https://github.com/waikato-ufdl/ufdl-backend
   cd ufdl-backend/docker/database

Then build the Dockerfile with custom username/password settings:

.. code:: bash

   docker build \
     -t ufdl_postgres \
     --build-arg UFDL_POSTGRESQL_USER=username \
     --build-arg UFDL_POSTGRESQL_PASSWORD=password \
     .

So that database data will persist between executions, create a volume for storage:

.. code:: bash

   docker volume create ufdl-pg

Run the image as a container:

.. code:: bash

   docker run \
    -p 5432:5432/tcp \
    -v ufdl-pg:/var/lib/postgresql/10/main \
    ufdl_postgres


Set up the backend
==================

The backend requires Redis to support web-socket connections to the server. Make sure a Redis server is installed
and running on the backend host.

Then, clone the following repositories (within the same directory):

* `ufdl-backend <Backend_>`__
* `ufdl-json-messages <JsonMessages_>`__

.. code:: bash

   git clone https://github.com/waikato-ufdl/ufdl-backend
   git clone https://github.com/waikato-ufdl/ufdl-json-messages

The backend requires instruction on how to connect to the PostgreSQL database, which is provided through environment
variables:

.. code:: bash

   export UFDL_POSTGRESQL_USER=username
   export UFDL_POSTGRESQL_PASSWORD=password
   export UFDL_POSTGRESQL_HOST=host.domain.name:port

The host defaults to *localhost* and the user to *ufdl*, so if these match your database configuration they need not
be supplied. The password has not default though and must be supplied.

Change into the directory of the cloned *ufdl-backend* repository and run the following script to set up
the virtual environment for the server (**CAUTION:** it will delete any previously stored data and the database):

.. code:: bash

   ./dev_init.sh

**N.B.**: *dev_init.sh* creates an admin user with username/password set to admin/admin respectively.

Once this has completed, you can start up the REST API on ``127.0.0.1`` as follows:

.. code:: bash

   ./dev_start.sh

Use ``0.0.0.0:8000`` as argument if you want to make the server available to the outside world on port 8000.
Ensure that your firewall allows that port to be accessed from the outside.


(Optional) Set up HTML front-end
================================

If you wish to use the HTML front-end with the UFDL system, it can be built and installed into the backend to be
served as a single-page application. Ensure you have Node installed, and then clone the required repositories
(within the same directory):

* `ufdl-ts-client <TypeScriptClient_>`__
* `ufdl-frontend-ts <HTMLFrontend_>`__

.. code:: bash

   git clone https://github.com/waikato-ufdl/ufdl-ts-client
   git clone https://github.com/waikato-ufdl/ufdl-frontend-ts

Build the client library:

.. code:: bash

   cd ufdl-ts-client
   npm install .
   npm run rebuild

Build the front-end:

.. code:: bash

   cd ../ufdl-frontend-ts
   npm install .
   npm run rebuild

Copy the built front-end into the backend for serving:

.. code:: bash

   cp -rf build /path/to/backend/venv.dev/lib/python3.7/site-packages/ufdl/html_client_app/static

The source clones for the client and front-end are no longer needed at this stage and can be safely deleted.


Set up the backend (Docker)
===========================

A Docker image with a preconfigured backend installation is also provided. This image also automatically includes the
HTML client ready-to-go. To obtain the image, with the Docker daemon running:

.. code:: bash

   docker login public.aml-repo.cms.waikato.ac.nz:443
   docker pull public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_backend:latest
   docker tag public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_backend:latest ufdl_backend

The default environment in this image is set to connect to a database on the Docker **host** (localhost) with
username/password both set to *ufdl*.

So that file data will persist between executions, create a volume for storage:

.. code:: bash

   docker volume create ufdl-fs

Run the image as a container:

.. code:: bash

   docker run \
    -v ufdl-fs:/ufdl/ufdl-backend/fs \
    --network=host \
    ufdl_backend

**N.B.** If the backend and the database are both running via Docker on the same machine, a private Docker network can
be created to allow the two services to communicate.

**N.B.** On setting up the backend for the first time, add that ``reset`` argument to the ``docker run`` command to
initialise the database with the requisite tables.


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


Set up a worker node (Docker)
=============================

A Docker image with a preconfigured worker node installation is also provided. To obtain the image, with the Docker
daemon running:

.. code:: bash

   docker login public.aml-repo.cms.waikato.ac.nz:443
   docker pull public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_job_launcher:latest
   docker tag public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_job_launcher:latest ufdl_job_launcher

Create a customised configuration file (as above) and then start the container with:

.. code:: bash

   docker run \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /path/to/job-launcher.conf:/ufdl/ufdl-job-launcher/examples/job-launcher-example.conf \
    -v /tmp/ufdl-job-launcher:/tmp/ufdl-job-launcher \
    --network=host \
    ufdl_job_launcher

**N.B.** If the backend and the database are both running via Docker on the same machine, a private Docker network can
be created to allow the two services to communicate.


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
.. _TypeScriptClient: https://github.com/waikato-ufdl/ufdl-ts-client
.. _ADAMSFrontend: https://adams.cms.waikato.ac.nz/snapshots/ufdl/
.. _HTMLFrontend: https://github.com/waikato-ufdl/ufdl-frontend-ts
.. _JobLauncher: https://github.com/waikato-ufdl/ufdl-job-launcher
