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


## Centralized Logging

### Do not start _centralized logging_ before ...

Actually do this from the start. Even if it is only a small log server or SaaS.

<details>
  <summary>Why?</summary>

- you always need log aggregation
- having centralized logging early on can defer having a monitoring system
</details>

### Do not start _SaaS-hosted centralized logging_ before ...

- you are clear on security implications
- you are sure your logs have only allowed user data (think GDPR, IP adresses...)
- you have good estimate on log volume that needs ingesting to your logging SaaS
- your network can handle the log ingest
- you monitoring log ingest per team to handle cases where suddenly debug traces are set to maximum cause large amounts of log lines

<details>
  <summary>Why?</summary>

- it is hard to predict ingest and growth
- SaaS services usually have cost per ingest volume
</details>

### Do not start _self-hosted centralized logging_ before ...

- you have good estimate on log volume
- you have good estimates on possible deduplication
- you know what retention you want to achieve
- you have a reliable storage backend (e.g. S3 appliance)
- you have a platform team guaranteeing uptime
- your network can handle the log ingest
- your network can handle catchup of log ingest after network outages
- you are able to separate high volume log traffic from latency relevant business logic
- you monitoring log ingest per team to handle cases where suddenly debug traces are set to maximum cause large amounts of log lines

<details>
  <summary>Why?</summary>

- log volume quickly becomes very large
- teams forget that they enabled debug tracing
</details>

## Monitoring

### Do not start _cloud-hosted monitoring_ (APM) before ...

- you run your platform in the cloud anyway
- cost is not an issue
- you do not have a platform/monitoring team
- you know what retention and which resolution intervals you want to achieve
- as long as your logging solution serves well

<details>
  <summary>Why?</summary>

- cost if usually billed by ingestion, teams tend to ingest a lot, costs explode
- you will need monitoring experts anyway and can avoid the lock-in
- not thinking about resolution intervals and relying simply on retention will lead to either
  - massive amount of data (when doing no downsampling)
  - lost information (when past incidents are downsampled away) 
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

## Mail

### Do not run _your own mailserver_ before

- you have enough time before go live to ensure it is widely trusted
- you have RBL monitoring in place
- you track the risk of not being able to send mail

<details>
  <summary>Why?</summary>

- building reputation for a new new mail domain takes a lot of send mails
- you need to start building it before going live with production user
- there is no guarantee that on go-live date the mail domain will be trusted
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
