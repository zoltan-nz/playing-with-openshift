# Playing with OpenShift (OKD)

OKD is the open source version of Red Hat's OpenShift. Previously OKD used to be called OpenShift Origin. OKD is a Platform As A Service solution and build on different component layers.

OKD combines together the following features ([ref][okd architecture overview])
:

- Docker Engine
- Kubernetes container orchestration
- Monitoring source code and build new docker images
- Deploy images
- Manage DNS and routing
- Load balancing and scaling up and down resources

## Motivation

Kubernetes is a powerful tool but it has a deep learning curve. One of the main benefit of OKD, that it provides a user friendly graphical user interface. Using Kubernetes via OKD helps to setup deployment and continuous integration much faster, so we allocate more time for application development.

## Getting started with OKD

We can install OKD on our development machine. We can use Minishift, a single-node OpenShift cluster management tool. Preferred way to install Minishift on macOS is using `brew`. ([ref][minishift installation])

*1. Setup `hyperkit` driver*

```shell
brew install hyperkit
brew install docker-machine-driver-hyperkit
```

*2. Install `minishift` with `brew cask`* 

```shell
brew cask install minishift
```

*3. Create and launch OKD Cluster*

```shell
minishift start --vm-driver hyperkit --disk-size 60GB --memory 6GB --cpus 4 --public-hostname cluster.local --network-nameserver 10.254.254.254
```

**Notes for the step 3:**

The terminal command above sets a custom public hostname. We have to add this hostname to `/etc/hosts` file. We need the IP address of the cluster also. The first time, the above process will exit, because it cannot find the host, however the cluster will be active. Run the following command. 

```shell
$ minishift ip
192.168.64.2
```

The ip address could be different. Setup `/etc/hosts`.

```
192.168.64.2 cluster.local
```

Additionally the custom DNS resolver (`dnsmasq`) should be also adjusted on the developer machine. Add the following address resolver line to `dnsmasq.conf`.  

```shell
address=/.cluster.local/192.168.64.2
```

Please note the `--network-nameserver` option set the host machine's localhost loopback alias address which is configured as a nameserver with `dnsmasq`.

**Stop and start `minishift`**

```shell
minishift stop
minishift start
```

**Open web console in browser**

```shell
minishift console
```

Default login details: `developer/developer` | `system/admin`



[okd architecture overview]: https://docs.okd.io/latest/architecture/index.html
[minishift installation]: https://docs.okd.io/latest/minishift/index.html