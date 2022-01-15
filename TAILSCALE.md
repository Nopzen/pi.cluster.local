# Tailscale VPN

This document is how to connect the nodes in a VPN and the exit node. For this part of the setup 
I'm using [Tailscale.io](https://tailscale.com/). Tailscale offers a Zero config VPN, using this
allows the cluster, to tie together devices accround devices and clouds to create a single network.

Once all the devices are connected to gether a Subnet will be deployed as a pod within the kubernetes cluster to cover the service ranges, so they later on can be exposed through the exit cloud node.

## Setting up the devices

After installing the tailscale binary run the following the command:

```
$ tailscale up --accept-routes
```

Follow the on screen instruction to finish setup.

It is important to include the flag `--accept-routes`, as this allows us to use the subnet router. 
That will be deployed in the kubernetes cluster.

## Deploying the subnet in Kubernetes

This will be the TL;DR if you want the long more indepth read i did this would be the link [Tailscale x Kubernetes](https://blog.porter.run/kubernetes-x-tailscale/).

1. Generate a authentication key on [Tailscale admin](https://login.tailscale.com/admin/settings/authkeys)
2. Get the ip range for the services: `kubectl get services`, example: `10.47.0.0/16`
3. Add the helm charts repo to helm: `helm repo add mvisonneau https://charts.visonneau.fr`
4. Install the pods.

```bash
helm install \
  tailscale-subnet-router \
  mvisonneau/tailscale-relay \
  --set config.authKey=<YOUR_AUTH_KEY> \
  --set config.variables.TAILSCALE_ADVERTISE_ROUTES=<YOUR_SUBNET_RANGE>
```

Once the subnet have been deployed, enable the subnet inside tailscale admin (see the write up above).
