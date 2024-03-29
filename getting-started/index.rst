.. title: Getting started
.. slug: getting-started
.. date: 2023-05-13 10:00:32 UTC+12:00
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

The server backend and worker nodes are expected to run Linux (tested with Ubuntu 20.04). The frontend,
e.g., when using ADAMS can be Linux, Windows or Mac. The HTML frontend has been tested with Chrome/Chromium and Firefox.

If you are on **Windows**, you should be able to run the backend/worker nodes as well, as long as
`WSL2 <https://learn.microsoft.com/en-us/windows/wsl/install>`__ is present.
See `here <https://www.data-mining.co.nz/applied-deep-learning/windows/>`__ for detailed instructions on getting
Docker configured (does not require the `Docker Desktop application <https://www.docker.com/products/docker-desktop/>`__,
as that may require license fees). Once that is in place, you can follow the steps below.

You can either use pre-configured Docker images or set up the system manually. See the respective section below
for relevant instructions.


Docker-Compose / Docker
+++++++++++++++++++++++

When using any of the public UFDL images, make sure to be logged into our registry:

.. code:: bash

   docker login public.aml-repo.cms.waikato.ac.nz:443


Docker-Compose
==============

The ``docker-compose`` script combines the three steps from the Docker section: **PostgreSQL**, **Redis**, **Backend**.
To get started with ``docker-compose``, first clone the backend repo:

.. code:: bash

   git clone https://github.com/waikato-ufdl/ufdl-backend
   cd ufdl-backend/docker/ufdl

Pull the latest images for the services:

.. code:: bash

   docker-compose pull

The **first time** running the system, the database will need to be initialised with the requisite tables:

.. code:: bash

   UFDL_RESET_BACKEND_ON_RUN=1 docker-compose up

**WARNING** The ``UFDL_RESET_BACKEND_ON_RUN`` environment variable should be un-set on subsequent runs, or previous
data will be erased! On subsequent runs, on-line the backend with the ``up`` command:

.. code:: bash

   docker-compose up


Docker
======

Instead of using ``docker-compose``, you can also execute the individual steps for setting up the backend.

PostgreSQL
----------

A Docker image which has a preconfigured PostgreSQL database is provided for convenience. To obtain the image, with
the Docker daemon running:

.. code:: bash

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

   docker run --rm \
    -p 5432:5432/tcp \
    -v ufdl-pg:/var/lib/postgresql/10/main \
    ufdl_postgres


Redis
-----

The backend requires access to a Redis server to enable the use of web-sockets. There is a publically-available image
for a Redis server available:

.. code:: bash

   docker pull public.aml-repo.cms.waikato.ac.nz:443/redis
   docker tag public.aml-repo.cms.waikato.ac.nz:443/redis:latest ufdl_redis

To run the image, only the port needs exposing:

.. code:: bash

   docker run --rm \
    -p 6379:6379 \
    ufdl_redis


Backend
-------

A Docker image with a preconfigured backend installation is also provided. This image also automatically includes the
HTML client ready-to-go. To obtain the image, with the Docker daemon running:

.. code:: bash

   docker pull public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_backend:latest
   docker tag public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_backend:latest ufdl_backend

The default environment in this image is set to connect to a database on the Docker **host** (localhost) with
username/password both set to *ufdl*. You can change these to match your database configuration via the ``--env``
option to ``docker run`` (below) an providing the environment variables described above, e.g.
``--env UFDL_POSTGRESQL_HOST=database.example.org``.

So that file data will persist between executions, create a volume for storage:

.. code:: bash

   docker volume create ufdl-fs

Start the backend for normal operation as follows:

.. code:: bash

   docker run --rm \
    -v ufdl-fs:/ufdl/ufdl-backend/fs \
    --name=ufdl_backend \
    --network=host \
    ufdl_backend

Before you can use the backend for the **first time**, you need to initialise the tables in the database:

.. code:: bash

   docker exec ufdl_backend ./dev_reset.sh


**NB:** If the backend and the database are both running via Docker on the same machine, a private Docker network can
be created to allow the two services to communicate.


Initialize
==========

* Download the ZIP file of the `ADAMS frontend <ADAMSFrontend_>`__ and unzip it.
* Start ADAMS with the ``bin/start_gui.sh`` script (Linux/Mac) or ``bin/start_gui.bat`` batch file (Windows).
* Use the *Flow editor* (from the *Tools* menu) to run the ``adams-ufdl-all-basic_setup.flow`` flow for setting up a
  basic environment (users, teams, projects).


Worker node (CPU)
=================

If you are using the Docker-Compose setup, a worker node can be started alongside the server with the
``with-job-launcher`` profile (requires docker-compose 1.28 or later):

.. code:: bash

   docker-compose --profile with-job-launcher up

A Docker image with a preconfigured worker node installation is also provided. To obtain the image, with the Docker
daemon running:

.. code:: bash

   docker pull public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_job_launcher:latest
   docker tag public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_job_launcher:latest ufdl_job_launcher

Download the `job-launcher-docker.conf <https://raw.githubusercontent.com/waikato-ufdl/ufdl-job-launcher/master/examples/job-launcher-docker.conf>`__
template and save it as something like ``/path/to/job-launcher.conf`` (you can adjust this path, of course).
Then you can launch the worker node as follows:

.. code:: bash

   docker run --rm \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /path/to/job-launcher.conf:/ufdl/ufdl-job-launcher/examples/job-launcher-example.conf \
    -v /tmp/ufdl-job-launcher:/tmp/ufdl-job-launcher \
    --network=host \
    ufdl_job_launcher

**NB:** 

* If the backend and the database are both running via Docker on the same machine, a private Docker network can be created to allow the two services to communicate.
* Since you are supplying the job launcher configuration to the docker container, make sure that the following directories are set to these values:

  * ``work_dir``: ``/tmp/ufdl-job-launcher``
  * ``cache_dir``: ``/tmp/ufdl-job-launcher/cache``


Worker node (GPU)
=================

If you want to make use of the GPU, you need to use ``with-job-launcher-gpu`` profile (requires docker-compose 1.28 or later):

.. code:: bash

   docker-compose --profile with-job-launcher-gpu up

A Docker image with a preconfigured worker node installation is also provided. To obtain the image, with the Docker
daemon running:

.. code:: bash

   docker pull public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_job_launcher-gpu:latest
   docker tag public.aml-repo.cms.waikato.ac.nz:443/ufdl/ufdl_job_launcher-gpu:latest ufdl_job_launcher-gpu

Download the `job-launcher-docker.conf <https://raw.githubusercontent.com/waikato-ufdl/ufdl-job-launcher/master/examples/job-launcher-docker.conf>`__
template and save it as something like ``/path/to/job-launcher.conf`` (you can adjust this path, of course).
Then you can launch the worker node as follows:

.. code:: bash

   docker run --rm \
    --gpus=all \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /path/to/job-launcher.conf:/ufdl/ufdl-job-launcher/examples/job-launcher-example.conf \
    -v /tmp/ufdl-job-launcher:/tmp/ufdl-job-launcher \
    --network=host \
    ufdl_job_launcher-gpu

**NB:**

* If the backend and the database are both running via Docker on the same machine, a private Docker network can be created to allow the two services to communicate.
* Since you are supplying the job launcher configuration to the docker container, make sure that the following directories are set to these values:

  * ``work_dir``: ``/tmp/ufdl-job-launcher``
  * ``cache_dir``: ``/tmp/ufdl-job-launcher/cache``



Manual setup
++++++++++++

Prerequisites
=============

Makes sure you have a valid development environment set up:

.. code:: bash

   sudo apt install build-essential python3-dev virtualenv


PostgreSQL
==========

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


Backend
=======

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
be supplied. The password has no default though and must be supplied.

Change into the directory of the cloned *ufdl-backend* repository and run the following script to set up
the virtual environment for the server (**CAUTION:** it will delete any previously stored data and the database):

.. code:: bash

   ./dev_init.sh

**NB:** *dev_init.sh* creates an admin user with username/password set to admin/admin respectively.

Once this has completed, you can start up the REST API on ``127.0.0.1`` as follows:

.. code:: bash

   ./dev_start.sh

Use ``0.0.0.0:8000`` as argument if you want to make the server available to the outside world on port 8000.
Ensure that your firewall allows that port to be accessed from the outside.


HTML front-end (optional)
=========================

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


Initialize
==========

* Download the ZIP file of the `ADAMS frontend <ADAMSFrontend_>`__ and unzip it.
* Start ADAMS with the ``bin/start_gui.sh`` script (Linux/Mac) or ``bin/start_gui.bat`` batch file (Windows).
* Use the *Flow editor* (from the *Tools* menu) to run the ``adams-ufdl-all-basic_setup.flow`` flow for setting up a
  basic environment (users, teams, projects).


Worker node
===========

On the worker node, clone the following repositories (within the same directory):

* `ufdl-json-messages <JsonMessages_>`__
* `ufdl-python-client <PythonClient_>`__
* `ufdl-job-launcher <JobLauncher_>`__
* `ufdl-annotations-plugin <AnnotationsPlugin>`__
* `ufdl-job-types <JobTypes>`__
* `ufdl-job-contracts <JobContracts>`__

.. code:: bash

   git clone https://github.com/waikato-ufdl/ufdl-json-messages
   git clone https://github.com/waikato-ufdl/ufdl-python-client
   git clone https://github.com/waikato-ufdl/ufdl-job-launcher
   git clone https://github.com/waikato-ufdl/ufdl-annotations-plugin.git
   git clone https://github.com/waikato-ufdl/ufdl-job-types.git
   git clone https://github.com/waikato-ufdl/ufdl-job-contracts.git

Change into the directory of the cloned *ufdl-job-launcher* repository and run the following script to set up
the virtual environment:

.. code:: bash

   ./dev_init.sh

In the ``examples`` directory, you can copy the ``job-launcher-example.conf`` configuration to ``job-launcher.conf``
and then update the required parameters (if anything, should be only the ``url``).

Once this suits your system, you can start the job-launcher like this (from within the ``ufdl-job-launcher`` directory):

.. code:: bash

   ./venv.dev/bin/ufdl-joblauncher -C examples/job-launcher.conf -c


Use the system
++++++++++++++

HTML Frontend
=============

Some of the functionality is available through a web-based frontend.
By default, the interface is being served on the following URL:

`http://localhost:8000/v1/html <http://localhost:8000/v1/html>`__

ADAMS
=====

The following `ADAMS <ADAMSFrontend_>`__ flows (`snapshot download <ADAMSsnapshots_>`__) are available to manage
your datasets and run jobs (simply execute them with the *Flow editor*):

* `adams-ufdl-core-manage_backend.flow <https://github.com/waikato-ufdl/ufdl-frontend-adams/blob/master/adams-ufdl-core/src/main/flows/adams-ufdl-core-manage_backend.flow>`__ - for managing the backend, starting jobs, etc.
* `adams-ufdl-image-manage_image_classification_datasets.flow <https://github.com/waikato-ufdl/ufdl-frontend-adams/blob/master/adams-ufdl-image/src/main/flows/adams-ufdl-image-manage_image_classification_datasets.flow>`__ - manages image classification datasets
* `adams-ufdl-image-manage_objected_detection_datasets.flow <https://github.com/waikato-ufdl/ufdl-frontend-adams/blob/master/adams-ufdl-image/src/main/flows/adams-ufdl-image-manage_object_detection_datasets.flow>`__ - manages object detection datasets
* `adams-ufdl-speech-manage_speech_datasets.flow <https://github.com/waikato-ufdl/ufdl-frontend-adams/blob/master/adams-ufdl-audio/src/main/flows/adams-ufdl-speech-manage_speech_datasets.flow>`__ - manages speech datasets


.. _Backend: https://github.com/waikato-ufdl/ufdl-backend
.. _JsonMessages: https://github.com/waikato-ufdl/ufdl-json-messages
.. _PythonClient: https://github.com/waikato-ufdl/ufdl-python-client
.. _JavaClient: https://github.com/waikato-ufdl/ufdl-java-client
.. _TypeScriptClient: https://github.com/waikato-ufdl/ufdl-ts-client
.. _ADAMSsnapshots: https://adams.cms.waikato.ac.nz/snapshots/ufdl/
.. _HTMLFrontend: https://github.com/waikato-ufdl/ufdl-frontend-ts
.. _JobLauncher: https://github.com/waikato-ufdl/ufdl-job-launcher
.. _AnnotationsPlugin: https://github.com/waikato-ufdl/ufdl-annotations-plugin
.. _JobTypes: https://github.com/waikato-ufdl/ufdl-job-types
.. _JobContracts: https://github.com/waikato-ufdl/ufdl-job-contracts
.. _ADAMSFrontend: https://github.com/waikato-ufdl/ufdl-frontend-adams
