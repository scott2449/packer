# Packer

Packer is a tool for building identical machine images for multiple platforms
from a single source configuration.

Packer is lightweight, runs on every major operating system, and is highly
performant, creating machine images for multiple platforms in parallel.
Packer comes out of the box with support for creating AMIs (EC2), VMware
images, and VirtualBox images. Support for more platforms can be added via
plugins.

## Quick Start

First, get Packer by either downloading a pre-built Packer binary for
your operating system or [downloading and compiling Packer yourself](#developing-packer).

After Packer is installed, create your first template, which tells Packer
what platforms to build images for and how you want to build them. In our
case, we'll create a simple AMI that has Redis pre-installed. Save this
file as `quick-start.json`. Be sure to replace any credentials with your
own.

```json
{
  "builders": [{
    "type": "amazon-ebs",
    "access_key": "YOUR KEY HERE",
    "secret_key": "YOUR SECRET KEY HERE",
    "region": "us-east-1",
    "source_ami": "ami-de0d9eb7",
    "instance_type": "m1.small",
    "ssh_username": "ubuntu",
    "ami_name": "packer-quick-start {{.CreateTime}}"
  }]
}
```

Next, tell Packer to build the image:

```
$ packer build quick-start.json
...
```

Packer will build an AMI according to the "quick-start" template. The AMI
will be available in your AWS account. To delete the AMI, you must manually
delete it using the [AWS console](https://console.aws.amazon.com/). Packer
builds your images, it does not manage their lifecycle. Where they go, how
they're run, etc. is up to you.

## Documentation

Full, comprehensive documentation is viewable on the Packer website:

http://www.packer.io/docs

## Developing Packer

If you wish to work on Packer itself, you'll first need [Go](http://golang.org)
installed (version 1.1+ is _required_). Next, clone this repository then just type `make`.
In a few moments, you'll have a working `packer` executable:

```
$ make
...
$ bin/packer
...
```

You can run tests by typing `make test`. This will run tests for Packer core
along with all the core builders and commands and such that come with Packer.
