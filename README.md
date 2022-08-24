# Introduction

The source control synchronizer is a straight forward container that will ensure bi-directional synchronization between a svn and a git repository. To keep it easy and straight forward the only branches which are synchronized (by default) are `svn:trunk` and `git:master`. The synchronization process is driven by a set of webhooks which can be called to trigger a synchronization.

# Running the container

```
$ docker run --detach source-control-synchronizer   \
    --svn-repository <svn remote uri>               \
    --git-repository <git remote uri>
```

# Hooking it in to the master repositories

Depending on your usage this might be different for you. I'm using this to synchronize between a bare bones SVN repository and a GIT repo hosted using GitLab.

## Setting up the hook on the SVN repo

## Setting up the hook on GitLab

# Implementation details