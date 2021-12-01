# DigitalOcean-Kubernetes-Challenge
This is my repo for the [DigitalOcean Kubernetes Challenge](https://www.digitalocean.com/community/pages/kubernetes-challenge)
If you find any inaccuracies in my explanations or useless / wrong configuration please let me know in an issue so I can learn from my mistakes :)



## Goals
My goals for this project were the following:
Learn how to use kubernetes and understanding how to use helm.

- Installing an image repository (Harbor)
- Installing and understanding ingresses
- Stapling more tech onto it for a functioning cluster

## What I've learned
- How to use digitalocean's kubernetes service.
- Using helm charts to install and override values.
- That Harbor is really cool, relatively "easy" to get started with and looks amazing.
- Installing and using traefik as an ingress controller

## Tech discovered
Harbor is pretty amazing, I've searched for registries in the past and Harbor was relatively painless and looks amazing.

## Mistakes I've made
Don't forget to add labels to your pods, I spend hours trying to figure out why the service selector wasn't working.

Helm upgrading with different encryption keys can lead to some weird behaviour (please correct me if I'm wrong)


## Setup

### Setup Traefik Ingress
```sh
helm install \
--namespace traefik \
--create-namespace \
--values charts/traefik/values.yml \
traefik traefik/traefik
```

### Setup Harbor (Don't forget to change the username/password combination)

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

### Example nginx website
`kubectl apply -f yml/nginx-example.yml`

### Local traefik dashboard at [traefik.localhost](http://traefik.localhost/dashboard/)
`kubectl apply -f yml/traefik-dashboard.yml`

This was only useful in local development and I used `kubectl port-forward` to debug in "prod"

## Deployed Examples
- [Example nginx](https://nginx.do.tortle.tech)
- [Harbor](https://harbor.do.tortle.tech)

## Resources

- https://doc.traefik.io/traefik/
- https://helm.sh/docs/
- https://kubernetes.io/docs/home/
- https://goharbor.io/docs/2.4.0/install-config/harbor-ha-helm/
- https://cert-manager.io/docs/