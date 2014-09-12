# An environment for Ceph training sessions using Vagrant

## Purpose

This repository allows you to spin up a Ceph training environment in
minutes. It deploys a total of 5 nodes using any distribution for
which supported packages are available from http://ceph.com, and
pre-installs the `ceph` and `ceph-deploy` packages. From there, you
can go ahead and deploy a virtualized Ceph mini-cluster.

## Requirements

You'll obviously need Vagrant. Since the Vagrant environment uses
Ansible for provisioning, you'll need that too. And then there are a
few extra requirements in terms of Vagrant plugins.

* `vagrant-hosts` is required so the nodes can resolve each others'
  host names.
* If you want to deploy on libvirt/KVM (recommended if your host runs
  Linux), you'll need `vagrant-libvirt` version 0.0.19 or higher.
* Since most Vagrant boxes are provided for the default `virtualbox`
  provider, to convert them to a format suitable for libvirt/KVM
  you'll also need `vagrant-mutate`.

## Configuration

You can tweak your deployed environment by modifying `settings.yml`

```yaml
---
ram: 1024

ceph_release: firefly

box: centos-7.0
# box: centos-6.5
# box: precise-server-cloudimg-amd64
# box: debian-7.6.0-amd64

box_url:  "https://vagrantcloud.com/chef/centos-6.5/version/1/provider/virtualbox.box"
```

* `ram` sets the amount of virtual RAM to allocate to each of the 5
nodes.

* `ceph_release` should be the codename of any supported Ceph release
  for which ceph.com hosts binary packages.

* `box` is the name of the box you want to deploy. Check with `vagrant
  box list` to see which boxes are currently available on your system.

* `box_url` is a publicly available URL from whence to download your
  box, if it hasn't already been downloaded.


## Converting boxes for your provider

Most published boxes are only available for the default `virtualbox`
provider. However, with the `vagrant-mutate` plugin installed,
converting such a box to one that Libvirt/KVM can use is easy. Suppose
you want to grab the `centos-6.5` box and make it available to
Libvirt:

```bash
vagrant box add centos-6.5 "https://vagrantcloud.com/chef/centos-6.5/version/1/provider/virtualbox.box"
vagrant mutate centos-6.5 libvirt
```

## Firing up the environment

Once you have adjusted `settings.yml` to your liking, create and
provision your boxes with

* `vagrant up`

or

* `vagrant up --provider=libvirt`

If you only want to bring up a single box or a few, you can do so with

* `vagrant up [--provider=libvirt] {alice|bob|charlie|daisy|eric|frank}`

