coreos-kubernetes-digitalocean
==============================

[These notes are now 25 days old -- a lifetime in a fast moving project like Kubernetes.  Check the Kubernetes Github repo for more up-to-date instructions before attempting to follow this.]

Configuration for running Kubernetes/CoreOS/Rudder on Digital Ocean

## Use this guide to get a Kubernetes cluster running on CoreOS linux on your Digital Ocean account.

This guide is far from perfect, and can be probably be improved/automated quite a bit.  Please send pull requests for corrections, clarifications and omissions.

## Step 1
You're going to need a Digital Ocean account.  If you don't have one, click [here](https://www.digitalocean.com/?refcode=9dd266a276e6) and use my referral link to get you (and me!) some free credit on Digital Ocean.  I host all the Gopher Academy and GopherCon sites on Digital Ocean, so your referral credits will be put to good use.

## Step 2

Follow [this guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-coreos-cluster-on-digitalocean) to install a three node CoreOS Cluster on Digital Ocean.  I named my servers "master", "minion1" and "minion2" instead of the recommended "coreos-1", "coreos-2", and "coreos-3" so that I would remember which machines had which responsibilities.

## Step 3

Go to the Digital Ocean control panel and write down the public and private IP addresses of all three machines.

Yours might look like this:

| Machine Name  | Public IP     | Private IP |
| ------------- |:-------------:| -----:|
| master    	| 104.131.x.1 	| 10.132.x.1 |
| minion1	| 104.131.x.2   |   10.132.x.1 |
| minion2	| 104.131.x.3   |    10.132.x.1 |

## Step 4

Install [Flannel](https://github.com/coreos/flannel) so that each pod in the Kubernetes cluster can have it's own IP address.

On one of the CoreOS machines, checkout the [Rudder source](https://github.com/coreos/rudder.git) and follow the [instructions](https://github.com/coreos/rudder#building-rudder) to use their docker container to build Rudder.

When you're done you should have Rudder installed at /opt/bin/rudder

## Step 5

Configure Rudder on each machine.

In /etc/systemd/system on each CoreOS machine, create a service file for rudder.  Use [this one](https://raw.githubusercontent.com/bketelsen/coreos-kubernetes-digitalocean/master/master/rudder.service) as a template.  Replace the line IP address my template with the correct PRIVATE IP address for that machine.  Remember you can get that by typing `ifconfig`.  My private IP addresses were on `eth1`.

Repeat this process for all three machines, ensuring that you use each machine's private ip address in the rudder.service file.

Add the service to systemctl:
`sudo systemctl enable /etc/systemd/system/rudder.service`

Reload systemctl:
`sudo systemctl daemon-reload`

Start Rudder:
`sudo systemctl start rudder`

## Step 6

Follow this same pattern to add `docker`, `kubelet`, and `proxy` services to all three machines.  Remember to use YOUR private IP address in the kubelet.service file.  Add each service, reload systemctl and start each service.  

## Step 7

On the master, add `apiserver` and `controller-manager`.  In the `apiserver.service` file, list the private IP addresses of all three CoreOS machines on [Line 15](https://github.com/bketelsen/coreos-kubernetes-digitalocean/blob/master/master/apiserver.service#L15u)

*** Important - you need to add a scheduler service now, too.  I'll try to add a service unit for this shortly. ***

## Step 8

Download `kubecfg` pre-built binaries by following the instructions at the bottom of [Kelsey Hightower's Guide](https://github.com/kelseyhightower/kubernetes-coreos)  I put mine in /opt/bin

With any luck you'll now have a fully operational Kubernetes cluster running on Digital Ocean.  To test it type `kubecfg list minions`.  You should see all three private ip addresses of your cluster listed in the result.

### Thanks/Credit

I couldn't have gotten the cluster running without lots of debugging help from Kelsey Hightower.  I also drew a lot of inspriation from [this post](https://translate.google.com/translate?sl=auto&tl=en&js=y&prev=_t&hl=en&ie=UTF-8&u=http%3A%2F%2Fqiita.com%2Fyungsang%2Fitems%2F530ae3d3277d2fba3343&edit-text=&act=url)


