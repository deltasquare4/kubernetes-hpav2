+++
title = "Using HorizontalPodAutoscaler V2 to autoscale HTTP services"
outputs = ["Reveal"]
+++

#### **Using `HorizontalPodAutoscaler` to autoscale HTTP services**

{{% note %}}
1m

* Who am I?
* How many of you have run applications on k8s?
* In production?
{{% /note %}}

---
## Auto(matic) scaling

* Why?
  * Reliability and Service Quality
  * Optimum resource usage
  * No human intervention required

{{% note %}}
3m

* Talk about how it was done before k8s
  * Pre-provisioning of instances based on past data - Wasteful
  * On-demand manual scaling - Human intervention needed
  * Autoscaling instances using custom image (AMIs) - makes deployment complex
* Kubernetes improves over this in every aspect
  * Efficient, smaller/quicker containers, no additional complexity
{{% /note %}}

---
## Scaling up/down in kubernetes

Scaling deployments manually is easy, and scriptable

```bash
$ kubectl scale deployment my-app --replicas=10
```
![Custom Autoscaler](/images/custom-autoscaler.png)

{{% note %}}
4m

* Explain how easy and scriptable scaling is in k8s
* Either one autoscaler per deployment, or custom tightly-integrated autoscaler
{{% /note %}}

---
## Enter `HorizontalPodAutoscaler`

![Horizontal Pod Autoscaler](/images/horizontal-pod-autoscaler.png)

{{% note %}}
5m

* K8s implements its own API that makes this easy without using custom scripts.
{{% /note %}}

---
## History

* V1: Autoscaling based on built-in metrics (cpu, memory) provided by heapster
* V2: Autoscaling based on default and custom metrics exported by metrics-server aggregated API

{{% note %}}
6m

{{% /note %}}

---
{{< markdown >}}
## Manifest

<!-- .slide: class="code" -->

```yaml
Kind: HorizontalPodAutoscaler
apiVersion: autoscaling/v2beta2
metadata:
  name: sample-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: sample-app
  minReplicas: 1
  maxReplicas: 5
  metrics:
    - type: Resource
      resource: cpu
      target:
        type: Utilization
        averageUtilization: 50
```
{{% note %}}
7m

{{% /note %}}

{{< /markdown >}}

---
## Custom Metrics?

* Collect through `Prometheus` ([https://prometheus.io](https://prometheus.io))
* Use `DirectXMan12/k8s-prometheus-adapter` ([https://github.com/DirectXMan12/k8s-prometheus-adapter](https://github.com/DirectXMan12/k8s-prometheus-adapter))

![Prometheus Adapter](/images/prometheus-adapter.png)

{{% note %}}
10m

* Prometheus collects and stores metrics from the cluster
* Prometheus Adapter fetches these metrics from prometheus and exposes it through k8s aggregated API

{{% /note %}}

---
# Demo

{{% note %}}
* Explain the application and the setup. minikube, prometheus operator, prometheus adapter, Node.js Hello World service. Show the code. Show empty prometheus.
* Deploy the Node.js deployment and service
* Deploy ServiceMonitor and see the metrics being collected
* Show the metrics being fetched from kubernetes raw API
* Show and Deploy HPA manifest
* Add watch on HPA
* Generate load on the service
* Watch them autoscale
* Scaling down will happen slowly, so Q/A while we wait.
* Scaled down. Thank you.

{{% /note %}}

---
### Thanks!

Code: [https://github.com/deltasquare4/k8s-http-autoscaling-demo](https://github.com/deltasquare4/k8s-http-autoscaling-demo)

Rakshit Menpara ([@deltasquare4](https://twitter.com/deltasquare4))

[improwised.com](https://www.improwised.com)
