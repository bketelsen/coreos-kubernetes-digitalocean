coreos-kubernetes-digitalocean
==============================

Configuration for running Kubernetes/CoreOS/Rudder on Digital Ocean

## Use this guide to get a Kubernetes cluster running on CoreOS linux on your Digital Ocean account.

This guide is far from perfect, and can be probably be improved/automated quite a bit.

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


