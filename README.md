# istio-demo

Demonstration of istio

## Install

### Install cluster

```bash
kind create cluster
```

### Install metallb

```bash
kubectl apply -f metallb-native.yaml
kubectl apply -f metallb-config.yaml
```

### Install istio

```bash
export PATH=$PATH:./istio-1.15.1/bin
istioctl install --set profile=demo -y
```

### Label namespace

```bash
kubectl label namespace default istio-injection=enabled
```

### Install bookinfo demo application

```bash
cd istio-1.15.1
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```

### open application to outside traffic

```bash
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```

### Install addons ( kiali prometheus, jaeger, zipkin, ... )

```bash
kubectl apply -f samples/addons/
```

### Open dashboard

```bash
istioctl dashboard kiali
```

### Traffic fun

```bash
kubectl apply -f network/destination-rule-all.yaml
```

```bash
kubectl apply -f network/virtual-service-reviews-*
```

### Cleanup

```bash
kubectl delete -f istio-1.15.1/samples/bookinfo/platform/kube/bookinfo.yaml
kubectl delete -f istio-1.15.1/samples/bookinfo/networking/bookinfo-gateway.yaml
kubectl delete -f network/destination-rule-all.yaml
```

## Canary Updates

### Install flagger

```bash
kubectl apply -k flagger/istio
```

### Create new Gateway

```bash
kubectl apply -f flagger/gateway.yaml
```

### Install podinfo demo app

```bash
kubectl apply -k flagger/podinfo
```

### Create canary

```bash
kubectl apply -f flagger/podinfo-canary.yaml
```

### Rollout new version

```bash
kubectl set image deployment/podinfo podinfod=ghcr.io/stefanprodan/podinfo:6.0.1
```
