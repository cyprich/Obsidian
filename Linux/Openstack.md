# Openstack

[Openstack](https://www.openstack.org/) is a set of software components that provide common services for cloud infrastructure

I will be installing Openstack on Ubuntu 24.04 LTS, following [this tutorial](https://ubuntu.com/tutorials/install-openstack-on-your-workstation-and-launch-your-first-instance#2-install-openstack)

## Installing

Install openstack via snap!

```bash
sudo snap install openstack --channel 2024.1/candidate
```

Install dependencies

```bash
sunbeam prepare-node-script --bootstrap | bash -x && newgrp snap_daemon
```

Bootstrap the cloud

```bash
sunbeam cluster bootstrap --accept-defaults --role control,compute,storage
```

> It might take a while, it took like an hour on my machine
> After like an hour, it failed, so i am not continuing with this...

Configure the cloud

```bash
sunbeam configure --accept-defaults --openrc demo-openrc
```
