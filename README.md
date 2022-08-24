# Introduction

The source control synchronizer is a straight forward container that will ensure bi-directional synchronization between a svn and a git repository. To keep it easy and straight forward the only branches which are synchronized (by default) are `svn:trunk` and `git:master`. The synchronization process is driven by a set of webhooks which can be called to trigger a synchronization.

# Running the container

Before running the container in synchronization mode we will need to initialize the working repositories. These can live inside the container, on a docker volume or a directory on available to your docker host. Inside the container these are mounted under `/var/svn` and `/var/git` respectively, the `AUTHORS` file is located at `/var/AUTHORS`. With the initialization process we want to create two "roughly" synchronized repositories. Depending on the size of your repos this might take a while.

## Building the container

```
$ docker build --tag localhost/source-control-synchronizer --file ./Dockerfile.synchronizer .
```

## SVN is leading

I'm assuming this would be the common case, all we provide the uri to the remote SVN repo and the container will make sure to create a local svn repository and from that create a git repository. We'll use `git svn clone` to clone the repo locally and when we're finished push it to the remote URI.

```
$ docker run source-control-synchronizer svn-to-git \
    --svn-repository <svn remote uri>               \
    --git-repository <git remote uri>
```

To cut down on time it makes sense to provide some roughly synchronized repositories as this is something that can be done beforehand, stored on disk and mounted into the container. Significantly cutting down on the time we spend moving everything around. Not providing a git remote uri will mean we won't push to a remote git repo, an origin can obviously be added later.

```
$ docker run                               \
    --volume "<svn repo>:/var/svn"         \
    --volume "<git repo>:/var/git"         \
    source-control-synchronizer svn-to-git --svn-repository <svn remote uri>
```

## GIT is leading



## Running the container in synchronization mode

```
$ docker run --detach source-control-synchronizer   \
    --svn-repository <svn remote uri>               \
    --git-repository <git remote uri>
```

## In practice

After the everything has been initialized and everyone is synchronizing, committing and pushing in all directions I think it makes sense to make this part of a set of containers using a docker compose file.

```
#
# See the docker-compose.example.yaml file in this repository, it's also used to test this project (within reason)
#
version: 3

services:
    gitlab:
    synchronizer:
    svn:

volumes:
    gitlab-data:
    svn-data:
    synchronizer-svn:
    synchronizer-git:
```

# Hooking it in to the master repositories

Depending on your usage this might be different for you. I'm using this to synchronize between a bare bones SVN repository and a GIT repo hosted using GitLab.

## Setting up the hook on the SVN repo

## Setting up the hook on GitLab

# Implementation details