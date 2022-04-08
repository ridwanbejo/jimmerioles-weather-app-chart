# A. About the project

This is the dockerize version of [jimmerioles/progressive-weather-app](https://github.com/jimmerioles/progressive-weather-app) that you could run into your Kubernetes Cluster. I use Helm Chart approach to deploy it easily into Kubernetes.

It has some features such as:

- Autoscaling
- Customized rolling upgrade strategy
- Customized Ingress
- Customized resource requests and limit
- Customized health check
- Customized pod disruption budget
- Customized node affnity

Requirements:

- Kubernetes 1.23+
- Helm v3.8.0
- Kubernetes nodes with your own node selector or these selector as a starter:
	- `app-development=true`
	- `db-development=true`
	- `monitor-development=true`

# B. How-To Guide

## 1. Install locally

```
helm install jimmerioles-weather-app-chart jimmerioles-weather-app/ --values jimmerioles-weather-app/values.yaml
```

## 2. Upgrade the chart

```
helm upgrade jimmerioles-weather-app-chart jimmerioles-weather-app/ --values jimmerioles-weather-app/values.yaml
```

## 3. Uninstall

```
helm uninstall jimmerioles-weather-app-chart
```

# C. How does it works?

The containerized version source code is forked from [jimmerioles/progressive-weather-app](https://github.com/jimmerioles/progressive-weather-app) and hosted at [ridwanbejo/progressive-weather-app](https://github.com/ridwanbejo/progressive-weather-app). Then I publish the docker image to [DockerHub](https://hub.docker.com/r/ridwanbejo/jimmoriales-pwa).

You can customize your settings from values.yaml and passed it to this chart installation as mentioned in How-To Guide. 

I've tested the deployment by deploying the chart into K3s in my local machine. The autoscaling feature is also tested by performing `ab -n 1000000 -c 1000 http://localhost:31246` and I saw that the HPA is functioning well by scaling the web app from 5 pods into 10 pods. Here is sample of autoscaling logs:

```
Events:
  Type     Reason                        Age                From                       Message
  ----     ------                        ----               ----                       -------
  Normal   SuccessfulRescale             1h                 horizontal-pod-autoscaler  New size: 5; reason: All metrics below target
  Normal   SuccessfulRescale             111s               horizontal-pod-autoscaler  New size: 10; reason: cpu resource utilization (percentage of request) above target
```