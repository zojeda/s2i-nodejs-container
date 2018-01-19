R Shiny Docker images
====================

This repository contains the source for building R Shiny application as a reproducible Docker image using
[source-to-image](https://github.com/openshift/source-to-image).
Users can choose between RHEL and CentOS based builder images.
The resulting image can be run using [Docker](http://docker.io).


Versions
---------------
R versions currently provided are:
* [R 3.4.3](3.4.3)

CentOS versions currently supported are:
* CentOS7


Installation
---------------
To build a R Shiny image:
*  **CentOS based image**

    This image is available on DockerHub. To download it run:

    ```
    $ docker pull zojeda/s2i-r-shiny-centos7
    ```

    To build a R Shiny image from scratch run:

    ```
    $ git clone --recursive https://github.com/zojeda/s2i-r-shiny-centos7
    $ cd zojeda/s2i-r-shiny-centos7
    $ git submodule update --init
    $ make build TARGET=centos7 VERSIONS=3.4.3
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of R.**


Usage
---------------------------------

For information about usage of Dockerfile for R 3.4.3,
see [usage documentation](3.4.3/README.md).


Test
---------------------
This repository also provides a [S2I](https://github.com/openshift/source-to-image) test framework,
which launches tests to check functionality of a simple R Shiny application built on top of the s2i-nodejs image.


*  **CentOS based image**

    ```
    $ cd s2i-nodejs-container
    $ git submodule update --init
    $ make test TARGET=centos7 VERSIONS=8
    ```

**Notice: By omitting the `VERSIONS` parameter, the build/test action will be performed
on all provided versions of R Shiny.**


Repository organization
------------------------
* **`<r-version>`**

    * **Dockerfile**

        CentOS based Dockerfile.

    * **`s2i/bin/`**

        This folder contains scripts that are run by [S2I](https://github.com/openshift/source-to-image):

        *   **assemble**

            Used to install the sources into the location where the application
            will be run and prepare the application for deployment (eg. installing
            modules using install_dependencies.R, etc.)

        *   **run**

            This script is responsible for running the application, by using the
            application web server.

        *   **usage***

            This script prints the usage of this image.

    * **`contrib/`**

        This folder contains a file with commonly used modules.

    * **`test/`**

        This folder contains the [S2I](https://github.com/openshift/source-to-image)
        test framework with simple R Shiny echo server.

        * **`test-app/`**

            A simple R Shiny echo server used for testing purposes by the [S2I](https://github.com/openshift/source-to-image) test framework.

        * **run**

            This script runs the [S2I](https://github.com/openshift/source-to-image) test framework.

