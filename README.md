# pi.cluster.local

This repository serves as a place, where developers who are new to Raspberry PI & Kubernetes, easily can get started with configuring their own personal home cloud environment. It will list requirements, a list of products I use, links to the learning resources I personally used to set this up, and more.

I hope this will serve as an inspiration for people both newer and seasoned developers who whats to get into Kubernetes with the k3s from Rancher version.

In this repository, I will not try to demonstrate how to set up the cluster but have links to an in-depth setup of k3s, I have a few simple web applications to deploy, and also some tooling that helps with daily interactions with a Kubernetes cluster.

## Requirements

To get started you will need a machine where you can run a linux os.

My personal cluster setup consists of:

 - Master node: [Raspberry Pi 4 B, 4Gb](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)
 - Agent node (1): [Raspberry Pi 4 B, 4Gb](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)
 - Agent node (2): [Raspberry Pi 3 B+, 1Gb](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/)
 - A stable and fast internet connection, Currently mine are running on my home WIFI, i will how ever recommend cabled ethernet connection for a more stable connection.

 _Short note on Raspberry Pi PSU, they can be rather expensive the official ones but are kinda important, to ensure your pi's will get the power needed, the problem is with phone chargers most of them do not deliver the ampere needed, most will deliver 1 - 2.4 amp, where the pi's will need 3 AMP, and 5 Volts, but most chargers will deliver that. :)_

Optional things
 - Master node GPIO Screen: [Pimoroni Hyper Pixel 4](https://shop.pimoroni.com/products/hyperpixel-4?variant=12569485443155)
 - Cluster rack: [GeekPi Raspberry Pi Cluster Rack](https://www.amazon.com/GeeekPi-Cluster-Raspberry-Heatsink-Stackable/dp/B07MW24S61/)

 None of the links above are referal or affiliate links, it's just links to the products I use.

## Resources

Here is a list of resources used while I learned how to set up my cluster.

I highly recommend if this is your first cluster to read the _Walk-through — install Kubernetes to your Raspberry Pi in 15 minutes by Alex Ellis_ as your entry point.

 - [Will it cluster? k3s on your Raspberry Pi (By Alex Ellis)](https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/)
 - [Walk-through — install Kubernetes to your Raspberry Pi in 15 minutes by Alex Ellis](https://alexellisuk.medium.com/walk-through-install-kubernetes-to-your-raspberry-pi-in-15-minutes-84a8492dc95a)
 - [Official K3S Docs by Rancher](https://rancher.com/docs/k3s/latest/en/)
 - [Official Kubernetes Docs by](https://kubernetes.io/)
 - [Tailscale x Kubernetes VPN](https://blog.porter.run/kubernetes-x-tailscale/)

## Tools

Here is a list of tools used by me to maintain my k3s cluster, most of them are found in the Blog posts from Alex above.

 - [arkade - The Open Source Kubernetes Marketplace](https://github.com/alexellis/arkade)
 - [kubectl - The commandline interface to ineract with the cluster](https://kubernetes.io/docs/tasks/tools/)
 - [k3sup - Commandline interface to easily install nodes and connect them](https://github.com/alexellis/k3sup)
 - [helm - The Kubernetes package manager](https://helm.sh/)

## Side notes

1. Once your master node is on your network a nice convenient thing to do would be to assign a domain in your `/etc/host` file, personally I use `pi.cluster.local`, using a `.local` domain is a nice little get around for your edge/chrome/firefox browser to have a little more relaxed rules towards non-HTTPS websites. To update your host file run the following command:

**Linux/OSX:** `sudo nano /etc/host`

**Windows:** Press `win + r` buttons to start `Run` and enter: `%WinDir%\System32\Drivers\Etc\host` and select a text editor of your choice to edit the file with.

Inside the host file, add your master node network IP followed by your decided `.local` domain. save the file. Go to your terminal and write: `ping <your-domain>.local` and you should see something like this:

```shell
$ ping pi.cluster.local
PING pi.cluster.local (machine-ip) 56(84) bytes of data.
64 bytes from pi.cluster.local (machine-ip): icmp_seq=1 ttl=63 time=3.43 ms
64 bytes from pi.cluster.local (machine-ip): icmp_seq=2 ttl=63 time=4.09 ms
64 bytes from pi.cluster.local (machine-ip): icmp_seq=3 ttl=63 time=3.40 ms
64 bytes from pi.cluster.local (machine-ip): icmp_seq=4 ttl=63 time=7.43 ms
```

If you do not see this, either try and restart your machine to ensure it picks up the changes to the host file, if that does not work, you might have written the wrong IP that it tries to resolve the domain towards.

1. A small side on running kubectl in WSL (Windows Subsystem for Linux), if you will ever need to use the `kubectl proxy` or the `kubectl port-forward` commands, remember to bind the `127.0.0.1` address to `0.0.0.0` with the flag `--address 0.0.0.0`, same flag applies to both commands if you bind the port on the `127.0.0.1/localhost` the networking layer in WSL won't pick up the port forwarding and you won't be able to connect to the cluster via browser, curl or postman (examples) _if someone has a fix for this let me know :D_

## Special thanks

I would like to give a special thanks to [Alex Ellis](https://twitter.com/alexellisuk) To have created a ton of great blog posts about k3s and pi clustering. He is a really nice guy, and really passionate about k3s and tolling for k3s, most of the tools above a created by Alex.
