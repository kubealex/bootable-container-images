# Introducing bootable containers!

- [What are bootable containers?](#what-are-bootable-containers-)
  * [How are they different?](#how-are-they-different-)
- [Let's get started](#let-s-get-started)
- [Use Cases](#use-cases)
- [Resources](#resources)

## What are bootable containers?

As the name suggests, bootable containers differ from general purpose containers as the underlying image they use contains all necessary bits to make it act exactly like a standard OS, including modules, systemd, etc.

### How are they different?

Bootable containers can enable a 360° integrated container-native workflow that covers the underlying OS up to the application layer.
They benefit of a set of dedicated tools (bootc, bootc-image-builder, etc) to compose, build and distribute images that can be defined using common Containerfile structure and instructions.

Some of the main features are:

- Bootable containers goal is to act as a traditional OS, initializing all components upon container start.
- Bootable containers images can act as a source to build VMs/Cloud images.
- Bootable containers images can be used as a source to install and configure a new server/VM using kickstart/Anaconda
- Bootable containers simplify testing applications on different architectures/platforms
- Bootable containers streamline OS updates leveraging the [rpm-ostree system](https://coreos.github.io/rpm-ostree/)

## Let's get started

Creating a bootable container is as easy as writing and running a Containerfile like this:

```dockerfile
FROM quay.io/centos-bootc/centos-bootc:stream9
CMD [ "/sbin/init" ] # optional, to init the system upon container start-up.
```

You can proceed customizing the image, adding users, packages, configurations, etc following the [Dockerfile Reference](https://docs.docker.com/reference/dockerfile/)


## Use Cases

In this repo you will find some use cases that explain and show bootable containers in action!

- [Simple bootc container](./use-cases/simple-bootc-container/)


## Resources

- [bootc project on GitHub](https://github.com/containers/bootc)
- [bootc documentation](https://containers.github.io/bootc/)
- [bootc-image-builder project on GitHub](https://github.com/osbuild/bootc-image-builder)

- [Podman page](https://podman.io/)
- [Fedora ELN project](https://docs.fedoraproject.org/en-US/eln/)
- [CentOS9 Stream project](https://centos.org/stream9/)