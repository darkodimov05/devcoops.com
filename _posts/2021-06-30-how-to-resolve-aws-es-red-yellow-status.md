---
layout: post
title:  "How to resolve AWS ElasticSearch cluster in red or yellow state"
categories: [ aws ]
image: assets/images/aws-es-state.jpg
tags: [aws, elk, elasticsearch]
---
If your ELK cluster has been deployed on AWS ElasticSearch managed service, and your position is to keep the cluster health green then you should follow the steps below to accomplish that. The `Cluster health` tab in your Amazon ES console indicates the status of the least healthy index in your cluster. So you may wonder why your status is `red` or `yellow`? 
- `red status:`  indicates that at least one primary shard and its replicas aren't allocated to a node
- `yellow status:` means that the primary shards for all indices are allocated to nodes in a cluster, but the replica shards for at least one index are not.
This tutorial will show you how to debug your cluster status and get it back in a green state.

## Prerequisites
* AWS Account
* Full access of the ElasticSearch Domain Service

## Troubleshooting the ES cluster
**Step 1**. First check if your ES cluster is in `red` or `yellow` state due to some `UNASSIGNED` shards:
```bash
curl -XGET 'ES_Endpoint/_cat/shards?h=index,shard,prirep,state,unassigned.reason' | grep UNASSIGNED
```
If there are `UNASSIGNED` shards the output should look like:
```bash
index1               1 r UNASSIGNED ALLOCATION_FAILED
index12              0 r UNASSIGNED ALLOCATION_FAILED
index11              5 r UNASSIGNED ALLOCATION_FAILED
index22              4 r UNASSIGNED NODE_LEFT
index25              2 r UNASSIGNED NODE_LEFT
```

**Step 2**. Executing the next command will identify the root cause of our cluster's unassigned shards:
```bash
curl -XGET 'ES_Endpoint/_cluster/allocation/explain'
```
Possible reasons:
```bash
"details": "failed shard on node [25kOkdsad15OPSre5fsa_g]: failed to create shard, failure IOException[failed to obtain in-memory shard lock]; nested: ShardLockObtainFailedException[[index11][1]: obtaining shard lock timed out after 5000ms, previous lock details: [shard creation] trying to lock for [shard creation]];

allocate_explanation": "cannot allocate because allocation is not permitted to any of the nodes"

"explanation": "shard has exceeded the maximum number of retries [5] on failed allocation attempts
```

**Step 3**. After finding the issue, we need to figure out which indices are causing our cluster to enter red/yellow status, so we can use the following query:
1. Red State
```bash
curl -XGET 'ES_Endpoint/_cat/indices?v&health=red'
```
`Output:`
```bash
health status index                                uuid                   pri rep docs.count docs.deleted store.size pri.store.size
red    open   index11                              9orZMYvPTA2TXR0UZIFyxA   5   0      55210        11870    302.2mb        302.2mb
red    open   index12                              1vKg-jGZSy2hA7-3H-qJrA   5   0     123580        29962    538.9mb        538.9mb
```
2. Yellow State
```bash
curl -XGET 'ES_Endpoint/_cat/indices?v&health=yellow'
```
`Output:`
```bash
health status index                        uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   index15                      xJ0KMwBiToagSwepcI663w   6   2     328022        54806    108.4mb         40.5mb
yellow open   index25                      2uqLbFH-Tl-Y5qCntiQVng   5   3      30280         5931    131.8mb         43.9mb
```

**Step 4**. The default value for re-allocating the shards is 5 which is not enough always, so we will have to increase the maximum retry setting with the following query for all red and yellow indices:
```bash
curl -XPUT 'ES_Endpoint/<indexname>/_settings'
{
"index.allocation.max_retries" : 10
}
```
This will trigger an ES API call where the leader node retries the shards allocation for a specified index on the cluster which will successfully assign that index.

**Step 5**. One more thing that you should check in your ES cluster settings is whether the number of replica shards for a specific index is matching the configured ES number. Let's say if you have configured `"number_of_replicas": "2"` for `index25` in your ES cluster and practically you have 3 or more, it will change the ES status, so you should make it as per your ES configuration:
```bash
curl -XPUT 'ES_Endpoint/<indexname>/_settings'
{
  "index" : {
    "number_of_replicas" : 2
  }
}
```

## Conclusion
If you closely follow the above steps, your AWS ElasticSearch cluster should get back to his previous green state.
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.