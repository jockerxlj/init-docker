Docker Version: 1.2.0
==================================
this branch is aimed for someone who is learning docker, interested 
in docker opensource and want to modify, build and run it.

![Docker L](docs/theme/mkdocs/images/docker-logo-compressed.png "Docker")

## Usage
build dev environment:
```bash
git clone https://github.com/jockerxlj/init-docker.git && \
    cd init-docker && git checkout dockerv1.2.0-dev
docker build -t dockerv1.2.0-dev .
```

run dockerv1.2.0-dev images:
```bash
docker run -it -v `pwd`:/go/src/github.com/docker/docker --privileged dockerv1.2.0-dev bash
```

in the container, you can modify source and build and install:
```bash
### build docker in /go/src/github.com/docker/docker/bundles/1.2.0/binary/docker-1.2.0
go build -o /go/src/github.com/docker/docker/bundles/1.2.0/binary/docker-1.2.0 -a -tags 'netgo static_build apparmor selinux daemon' -ldflags '
        -w
        -X github.com/docker/docker/dockerversion.GITCOMMIT "e6a3d62"
        -X github.com/docker/docker/dockerversion.VERSION "1.2.0"
        -linkmode external
        -X github.com/docker/docker/dockerversion.IAMSTATIC true
        -extldflags "-static -lpthread -Wl,--unresolved-symbols=ignore-in-object-files"
        ' ./docker

### you can run "go install" and the docker binary will install in /go/bin/
go install -a -tags 'netgo static_build apparmor selinux daemon' -ldflags '
        -w
        -X github.com/docker/docker/dockerversion.GITCOMMIT "e6a3d62"
        -X github.com/docker/docker/dockerversion.VERSION "1.2.0"
        -linkmode external
        -X github.com/docker/docker/dockerversion.IAMSTATIC true
        -extldflags "-static -lpthread -Wl,--unresolved-symbols=ignore-in-object-files"
        ' ./docker
```

run docker:
```
/PATH/TO/DOCKER -d -s=vfs -D
```

## Change logs compared to the dockerv1.2.0

一. modify some file in dockerv1.2.0 so that it can build more smoothly 

git-commit-id: e6a3d62d3f14c86aaca46193a5e3c004112f6d3f

  1. Modify Dockerfile: fix the timeout problems because of the GFW.
  2. Add some dubug information in hack/make.sh to make it easier to understand
     the building process.
  3. Modify some "import pkg path" in order fo find pkg while building.

二. add pmtu discovery to find min-mtu in hosts 

related issue [click ere](https://github.com/moby/moby/issues/22297)

git-commit-id: 12d5cc91f258e62d01e93b00f9a474eb610c4810

  1. mainly modify: daemon/daemon.go, daemon/networkdriver/bridge/driver.go

### Legal

*Brought to you courtesy of our legal counsel. For more context,
please see the Notice document.*

Use and transfer of Docker may be subject to certain restrictions by the
United States and other governments.  
It is your responsibility to ensure that your use and/or transfer does not
violate applicable laws. 

For more information, please see http://www.bis.doc.gov


Licensing
=========
Docker is licensed under the Apache License, Version 2.0. See LICENSE for full license text.

