+++
title = "Using HorizontalPodAutoscaler V2 to autoscale HTTP services"
outputs = ["Reveal"]
+++

#### **Using `HorizontalPodAutoscaler` to autoscale HTTP services**

{{% note %}}
* Who am I?
* How many of you have run applications on k8s?
* In production?
* I need
{{% /note %}}

---
## Auto(matic) scaling

* Why?
  * Reliability and Service Quality
  * Optimum resource usage
  * No human intervention required

{{% note %}}
5m

* Talk about how it was done before k8s
{{% /note %}}

---
## Scaling up/down in kubernetes

Scaling deployments manually is easy, and scriptable

```bash
$ kubectl scale deployment my-app --replicas=10
```
![Custom Autoscaler](/images/custom-autoscaler.png)

---
## Enter `HorizontalPodAutoscaler`

![Horizontal Pod Autoscaler](/images/horizontal-pod-autoscaler.png)

---
## History

* V1: Autoscaling based on built-in metrics (cpu, memory) provided by heapster
* V2: Autoscaling based on default and custom metrics exported by metrics-server aggregated API

---
## Manifest
{{< markdown >}}
#### Misconceptions About a Programmer's Job

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
{{< /markdown >}}

---
## Custom Metrics?

* Collect through `Prometheus` ([https://prometheus.io](https://prometheus.io))
* Use `DirectXMan12/k8s-prometheus-adapter` ([https://github.com/DirectXMan12/k8s-prometheus-adapter](https://github.com/DirectXMan12/k8s-prometheus-adapter))

![Prometheus Adapter](/images/prometheus-adapter.png)

---
# Demo

---
### Thanks!

Rakshit Menpara ([@deltasquare4](https://twitter.com/deltasquare4))

[improwised.com](https://www.improwised.com)
