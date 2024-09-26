# Local development of a Jenkins Shared Library

This example shows how to develop and test Shared Libraries locally.

## Pre-requisites

* Docker and Docker compose must be available on your development machine.
* an environment variable LIBRARY must be set and point to a directory where
the library code exists.
* The directory where the library code is must be a Git repository

## Setup

This example project comes with a simple docker-compose.yaml file which uses a
public Jenkins docker image.

**Note** that the docker-compose.yaml maps a directory on the host that contains
the code of the shared library to test to a directory inside the container. Such
directory is then referred to by the CasC configuration (in the jenkins.yaml).

**Note** that the docker-compose.yaml defines a docker volume where the
`JENKINS_HOME` will be stored. If you ever want to start from scratch, run
`docker compose down -v` to destroy the container and the volume.

In order for this example to work Jenkins needs to have the following plugins
installed:

* [configuration-as-code](https://plugins.jenkins.io/configuration-as-code/)
* [pipeline](https://plugins.jenkins.io/workflow-aggregator/)
* [git](https://plugins.jenkins.io/git/)

### Bootstrap sequence

1. cd into the dir where the docker-compose.yaml is
1. export LIBRARY=...
1. docker compose up - run it with `-d` if you want it to run in background
1. from the docker compose logs copy the password to be used to unlock Jenkins
1. open the browser on http://localhost:8080 and paste the password in the text
box
1. select the option `select plugins to install`, then click `none` to deselect
the suggested plugins
1. search and install the plugins listed above by searching their name in the
search box and select them.
    1. git
    1. pipeline
    1. configuration as code
1. once the plugins have been installed follow through to the end of the wizard

Your Jenkins is ready to go.

Notice that the `system message` says `Test Shared Libraries` indicating that
the CasC definition of the `jenkins.yaml` has been loaded. Your library to test
should also be configured to point to `/var/library/.git`.

**Note**: By default the Git plugin would not allow to clone from a directory as
this could be a potential security risk. In order to allow that, we have to
define a specific system property which is set in the docker-compose.yaml:

```
hudson.plugins.git.GitSCM.ALLOW_LOCAL_CHECKOUT=true
```

As this is a testing environment, we allow ourselves to clone from a local dir.

## Configuration As Code

As mentioned, `jenkins.yaml` file provides the configuration of Jenkins as code,
including the configuration of the shared library that we want to test.

The code is configured to point to a local git repository, and the default
version is main, which means it will always pick the last committed change in
the local repo.





