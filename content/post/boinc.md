---
title: "Boinc"
date: 2024-08-11T15:38:28-04:00
draft: true
---

My friend Adam was telling me about worldcommunitygrid the other day (write a little about what that is).  It reminded me of [distributed.net](https://distributed.net), with arguably nobler causes behind the project.  I have a good chunk of unused cpu and gpu capacity laying around so I decided I'd set up the [boinc client]() on my homelab.

# linuxservers

I started by running a [copy of the container](https://github.com/linuxserver/docker-boinc) over at [linuxserver](https://www.linuxserver.io/) and that worked well for getting my feet wet.  It runs a _lot_ of stuff I don't really want or need and I wasn't sure how I could automatically join the pod to my account.  Adam showed me how he configures his machines to get to work so I decided I needed to do something similar for my setup.

# make my own container

Inspired by [Adam's ansible task](https://github.com/maxamillion/maxible/blob/main/roles/worldcommunitygrid/tasks/main.yml#L21-L28), I set out to [build my own container](https://github.com/jhjaggars/boinc) and run it.  It turned out to be really simple.  You can find it under [my personal quay.io account](https://quay.io/jhjaggars/boinc).

The resulting PodSpec looks kind of like this:

```
      containers:
        - name: boinc
          image: quay.io/jhjaggars/boinc:latest
          resources:
            requests:
              memory: "100Mi"
              cpu: "10m"
            limits:
              cpu: "6000m"
          env:
          - name: BOINC_URL
            value: "www.worldcommunitygrid.org"
          - name: BOINC_ACCOUNT_KEY
            value: "1169799_9ee82bab35a6ad9683c654aabfd8882b"
          volumeMounts:
          - name: data
            mountPath: /boinc-data
```

That `ACCOUNT_KEY` is a so-called _weak_ key that can only be used to add machines to my account.  So it's not really sensitive, and if you really want to spend your cpu cycles to credit my account, go right ahead :).

# observability

One of the things I wanted immediately was to monitor my progress.  I found [this project](https://gitlab.com/ordaa/boinc_exporter) from a [reddit post](https://www.reddit.com/r/BOINC/comments/lpb0tz/monitor_your_boinc_installations_with_prometheus/).  The way the exporter works is that it writes the metrics to a file to be collected by the node exporter which isn't what wanted to do.  I decided to write my own exporter that could be scraped by a [PodMonitor](https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/api.md#monitoring.coreos.com/v1.PodMonitor).

Adding my new exporter as a sidecar looks like this:

```
      initContainers:
        - name: metrics-exporter
          image: quay.io/jhjaggars/boinc-exporter:latest
          restartPolicy: Always
          env:
            - name: BOINC_CLIENT_STATE_XML
              value: "/boinc-data/client_state.xml"
          ports:
            - containerPort: 9100
              name: metrics
          volumeMounts:
            - name: data
              mountPath: /boinc-data
```

And the PodMonitor I'm using to scrape said sidecar looks like this:

```
apiVersion: monitoring.coreos.com/v1
kind: PodMonitor
metadata:
  name: boinc-monitor
  namespace: boinc
  labels:
    release: kube-prometheus-stack
spec:
  selector:
    matchLabels:
      app: boinc
  podMetricsEndpoints:
    - port: metrics
      path: /metrics
```

My project is a work-in-progress but you can try it out if you like [here](https://github.com/jhjaggars/boinc-exporter).

# gpus

In order to take advantage of the onboard GPUs that my NUCs have I installed the [Intel Device Plugins for Kubernetes](https://intel.github.io/intel-device-plugins-for-kubernetes/cmd/gpu_plugin/README.html#install-with-operator) and the gpu plugin.

In order to get my boinc container to utilize the onboard gpu I needed to add the `intel-opencl` package.

I added the following to the Pod Spec:

```
...
        limits:
          gpu.intel.com/i915: "1"
...
```


# statefulset

I wanted to run the boinc client on all of my nuc machines so I scaled my initial deployment up to 3 and set an anti-affinity rule to prevent multiple copies from running on the same node.  I ran into a storage issue immediately, which should have been obvious.  Lucky for me, this is what [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)s are made to help with.  Now each pod gets it's own volume according to my template and I can effectively run 3 clients at once each working on their own jobs.

My manifest looks like this now:

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: boinc
  namespace: boinc
spec:
  replicas: 3
  selector:
    matchLabels:
      app: boinc
  template:
    metadata:
      labels:
        app: boinc
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                  - key: app
                    operator: In
                    values:
                    - "boinc"
              topologyKey: kubernetes.io/hostname
      initContainers:
        - name: metrics-exporter
          image: quay.io/jhjaggars/boinc-exporter:latest
          restartPolicy: Always
          env:
            - name: BOINC_CLIENT_STATE_XML
              value: "/boinc-data/client_state.xml"
          ports:
            - containerPort: 9100
              name: metrics
          volumeMounts:
            - name: data
              mountPath: /boinc-data
      containers:
        - name: boinc
          image: quay.io/jhjaggars/boinc:latest
          resources:
            requests:
              memory: "100Mi"
              cpu: "10m"
            limits:
              cpu: "6000m"
              gpu.intel.com/i915: "1"
          env:
          - name: BOINC_URL
            value: "www.worldcommunitygrid.org"
          - name: BOINC_ACCOUNT_KEY
            value: "1169799_9ee82bab35a6ad9683c654aabfd8882b"
          volumeMounts:
          - name: data
            mountPath: /boinc-data
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes: ["ReadWriteOnce"]
        storageClassName: "local-path"
        resources:
          requests:
            storage: 1Gi
```

# join the team

I thought it'd be fun to start a team and Adam agreed so we are now working together as the ['grouchy nerds'](https://www.worldcommunitygrid.org/team/viewTeamInfo.do?teamId=33X5Z8GRL2) on worldcommunitygrid.  Feel free to join and share your progress to make the numbers go up! 
