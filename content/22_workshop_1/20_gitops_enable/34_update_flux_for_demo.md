---
title: "Speed up Flux for Demo"
chapter: false
weight: 34
---

By default Flux will poll git every 5m but for the sake of the demo, we want to speed things up a bit.

Lets edit the flux deployment and add the following argument into around line 180 `--git-poll-interval`


Edit the following file in your prefered way:

```
./flux/flux-deployment.yaml
```

```yaml
175      # Serve /metrics endpoint at different port;
176      # make sure to set prometheus' annotation to scrape the port value.
177      - --listen-metrics=:3031
178
179      # Additional arguments
180      - --sync-garbage-collection
190      - --git-poll-interval=1m
```
