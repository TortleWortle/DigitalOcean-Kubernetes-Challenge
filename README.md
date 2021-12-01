# DigitalOcean-Kubernetes-Challenge
https://www.digitalocean.com/community/pages/kubernetes-challenge


## Goals
My goals for this project were the following:
Learn how to use kubernetes and understanding how to use helm.

Installing an image repository (Harbor)

Installing and understanding ingresses

Stapling more tech onto it for a functioning cluster

## What I've learned
How to use digitalocean's kubernetes service.

How to use helm charts, view possible values and overriding them

How to install traefik as an ingress and using the kubernetes ingress api

How to install harbor and partially configuring it


# Setup

## Setup Traefik Ingress
```sh
helm install \
--namespace traefik \
--create-namespace \
--values charts/traefik/values.yml \
traefik traefik/traefik
```

## Setup Harbor (Don't forget to change the username/password combination)

Generate Initial Admin password and view it
```sh
HARBORPASS=$(openssl rand -hex 8)
echo $HARBORPASS
```

```sh
HARBORKEY=$(openssl rand -hex 8)
helm install \
--namespace harbor \
--create-namespace \
--values charts/harbor/values.yml \
--set-string "secretKey=$HARBORKEY" \
--set-string "harborAdminPassword=$HARBORPASS" \
harbor harbor/harbor
```

Don't forget to use the same secretKey when doing a `helm upgrade`. Because I did and everything broke :)

## Local traefik dashboard at [traefik.localhost](http://traefik.localhost/dashboard/)
`kubectl apply -f yml/traefik-dashboard.yml`


## Samples
- [Example nginx](https://nginx.do.tortle.tech)
- [Harbor](https://harbor.do.tortle.tech)

## Resources

- https://doc.traefik.io/traefik/
- https://helm.sh/docs/
- https://kubernetes.io/docs/home/
- https://goharbor.io/docs/2.4.0/install-config/harbor-ha-helm/
- https://cert-manager.io/docs/