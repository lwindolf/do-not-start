# Do not start X before ...

This is a collection of criteria for not doing DevOps thing X before Y is given.
These rules a in my opinion good definitions of threshold which defines when
you want to introduce a certain type of complexity in your org+IT system. The criteria
given here can be thought of as input for [architectural decision records](https://github.com/adr/madr).

## kubernetes

### Do not start _cloud-hosted k8s_ before ...

- dev team >=10 people

<details>
  <summary>Why?</summary>

- smaller dev teams will not be able to build the knowledge
- smaller dev teams will lack distributed knowledge

Further reading

- https://dev.to/synsun/kubernetes-vs-docker-swarm-vs-nomad-container-orchestration-showdown-2026-532e
  
</details>

### Do not start _self-hosted k8s_ before ...

- dev team >=10 people
- you have 1 platform engineer doing on-call (that means 3 platform engineers)
- you have SaaS APM monitoring or host it yourself with 2+ additional monitoring experts in your platform team

<details>
  <summary>Why?</summary>

- smaller dev teams will not be able to build the knowledge
- smaller dev teams will lack distributed knowledge
- platform engineers will have to constantly upgrade k8s components
- platform engineers needed to set and enforce k8s best practices

Further reading

- https://dev.to/synsun/kubernetes-vs-docker-swarm-vs-nomad-container-orchestration-showdown-2026-532e

</details>

### Do not start _self-hosted k8s quota/limits_ before ...

- you know how much overcommit you want to allow
- you know how much resource waste you want to accept

<details>
  <summary>Why?</summary>

- strict quotas usually cause very low cluster utilisation, wasting money
- optimize cost by not setting CPU limits
- consider only limiting RAM
- do extensive capacity monitoring including drilldowns per team
</details>

## Monitoring

### Do not start _cloud-hosted monitoring_ (APM) before ...

- you run your platform in the cloud anyway
- cost is not an issue
- you do not have a platform/monitoring team

<details>
  <summary>Why?</summary>

- cost if usually billed by ingestion, teams tend to ingest a lot, costs explode
- you will need monitoring experts anyway and can avoid the lock-in
</details>

### Do not start _self-hosted monitoring_ before ...

- you have 2 dedicated monitoring experts
- you have a platform team guaranteeing necessary uptime
- you know how to do reliable alerting

<details>
  <summary>Why?</summary>

- monitoring SW (grafana, prometheus) needs updating peridiocally
- data ingestion / compaction has a high failover rate
- teams need experts helping building dashboards
- teams need experts helping with query performance
</details>

## Hybrid cloud

### Do not start _a hybrid cloud_ before

- you have dedicated platform teams for cloud and on-prem
- you know how to integrate cloud+on-prem monitoring/alerting for use cases using both
- you know how much ingress/egress into the cloud this means
- you know the throughput of you network into the cloud
- you have cloud ingress cost monitoring automated

<details>
  <summary>Why?</summary>

- cloud ingress is usually very expensive
- cloud ingress might not scale well at all
- failover for cloud ingress is excessively expensive, it usually stays a SPoF
</details>
