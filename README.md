NodeJS for DeployDock - Docker images
========================================

This repository contains the source for building various versions of
the Node.JS application as a reproducible Docker image using
[source-to-image](https://github.com/Meros-io/source-to-image).
Users can choose between RHEL and CentOS based builder images.
The resulting image can be run using [Docker](http://docker.io).


Versions
---------------
Node.JS versions currently provided are:
* nodejs-0.10

RHEL versions currently supported are:
* RHEL7

CentOS versions currently supported are:
* CentOS7


Installation
---------------
To build a Node.JS image, choose either the CentOS or RHEL based image:
*  **RHEL based image**

    To build a RHEL based Node.JS-0.10 image, you need to run the build on a properly
    subscribed RHEL machine.

    ```
    $ git clone https://github.com/Meros-io/sti-nodejs.git
    $ cd sti-nodejs
    $ make build TARGET=rhel7 VERSION=0.10
    ```

*  **CentOS based image**

    This image is available on DockerHub. To download it run:

    ```
    $ docker pull deploydock/nodejs-010-centos7
    ```

    To build a Node.JS image from scratch run:

    ```
    $ git clone https://github.com/Meros-io/sti-nodejs.git
    $ cd sti-nodejs
    $ make build VERSION=0.10
    ```

**Notice: By omitting the `VERSION` parameter, the build/test action will be performed
on all provided versions of Node.JS. Since we are currently providing only version `0.10`,
you can omit this parameter.**


Usage
---------------------
To build a simple [nodejs-sample-app](https://github.com/Meros-io/sti-nodejs/tree/master/0.10/test/test-app) application
using standalone [STI](https://github.com/Meros-io/source-to-image) and then run the
resulting image with [Docker](http://docker.io) execute:

*  **For RHEL based image**
    ```
    $ s2i build https://github.com/Meros-io/sti-nodejs.git --context-dir=0.10/test/test-app/ deploydock/nodejs-010-rhel7 nodejs-sample-app
    $ docker run -p 8080:8080 nodejs-sample-app
    ```

*  **For CentOS based image**
    ```
    $ s2i build https://github.com/Meros-io/sti-nodejs.git --context-dir=0.10/test/test-app/ deploydock/nodejs-010-centos7 nodejs-sample-app
    $ docker run -p 8080:8080 nodejs-sample-app
    ```

**Accessing the application:**
```
$ curl 127.0.0.1:8080
```


Test
---------------------
This repository also provides a [S2I](https://github.com/Meros-io/source-to-image) test framework,
which launches tests to check functionality of a simple Node.JS application built on top of the sti-nodejs image.

Users can choose between testing a Node.JS test application based on a RHEL or CentOS image.

*  **RHEL based image**

    To test a RHEL7 based Node.JS-0.10 image, you need to run the test on a properly
    subscribed RHEL machine.

    ```
    $ cd sti-nodejs
    $ make test TARGET=rhel7 VERSION=0.10
    ```

*  **CentOS based image**

    ```
    $ cd sti-nodejs
    $ make test VERSION=0.10
    ```

**Notice: By omitting the `VERSION` parameter, the build/test action will be performed
on all provided versions of Node.JS. Since we are currently providing only version `0.10`
you can omit this parameter.**


Repository organization
------------------------
* **`<nodejs-version>`**

    * **Dockerfile**

        CentOS based Dockerfile.

    * **Dockerfile.rhel7**

        RHEL based Dockerfile. In order to perform build or test actions on this
        Dockerfile you need to run the action on a properly subscribed RHEL machine.

    * **`s2i/bin/`**

        This folder contains scripts that are run by [STI](https://github.com/Meros-io/source-to-image):

        *   **assemble**

            Used to install the sources into the location where the application
            will be run and prepare the application for deployment (eg. installing
            modules using npm, etc.)

        *   **run**

            This script is responsible for running the application, by using the
            application web server.

        *   **usage***

            This script prints the usage of this image.

    * **`contrib/`**

        This folder contains a file with commonly used modules.

    * **`test/`**

        This folder contains the [S2I](https://github.com/Meros-io/source-to-image)
        test framework with simple Node.JS echo server.

        * **`test-app/`**

            A simple Node.JS echo server used for testing purposes by the [S2I](https://github.com/Meros-io/source-to-image) test framework.

        * **run**

            This script runs the [S2I](https://github.com/Meros-io/source-to-image) test framework.

* **`hack/`**

    Folder containing scripts which are responsible for the build and test actions performed by the `Makefile`.


Image name structure
------------------------
##### Structure: deploydock/1-2-3

1. Platform name (lowercase) - nodejs
2. Platform version(without dots) - 010
3. Base builder image - centos7/rhel7

Examples: `deploydock/nodejs-010-centos7`, `deploydock/nodejs-010-rhel7`


Environment variables
---------------------

To set environment variables, you can place them as a key value pair into a `.sti/environment`
file inside your source code repository.

Example: DATABASE_USER=sampleUser
