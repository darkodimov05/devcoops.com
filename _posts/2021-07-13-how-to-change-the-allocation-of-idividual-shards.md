---
layout: post
title:  "How to manually change the allocation of shards"
categories: [ ElasticSearch ]
image: assets/images/elasticshard.jpg
tags: [ aws, elk, elasticsearch ]
---
While updating the mapping on some of the ES clusters or doing a full reindex which are expensive operations, you may face a data node failure. Due to the node failure, there is a possibility some of the shards to become unassigned, but the situation will get complicated if you haven't configured a replica shard. This will correspond with a RED cluster state in AWS. You can mitigate it by ensuring that you have at least 1 replica, ideally if you have a sufficient number of replicas set up to survive the loss of N data nodes in the cluster. That way Elasticsearch keep one shard as the primary while it moves others around.

## Prerequisites
* Full access of the ElasticSearch Domain Service

## Checking the unassigned shards in the ES Cluster
**Step 1**. Check which shards are unassigned:
```bash
curl -XGET 'ES_Endpoint/_cat/shards?h=index,shard,prirep,state,unassigned.reason' | grep UNASSIGNED
```

**Step 2.** Identify the root cause of our cluster's unassigned shards
```bash
curl -XGET 'ES_Endpoint/_cluster/allocation/explain'
```
If you find the following `error`:
```bash
cannot allocate because a previous copy of the primary shard existed but can no longer be found on the nodes in the cluster
```
The meaning of the error is that this is the primary shard and there is no replica shard, so the `primary` shard cannot be reallocated because of data loss. One solution is to reindex those indexes or return them from backup, but the easiest and safest method is to manually reallocate the shards to one of the active data nodes and immediately increase the number of replicas at least one.

## Manually change the allocation of shards
**Step 1**. Execute the following reroute API call to manually reallocate the needed shard
```bash
curl -XPOST 'ES_Endpoint/_cluster/reroute'
{
  "commands": [
    {
      "move": {
        "index": "index-name", "shard": 0,
        "from_node": "node1", "to_node": "node2"
      }
    }
  ]
}
```

**Step 2**. After executing the command the shard should be initialized on the new node. So you will have to change the number of replica to at least one:
```bash
curl -XPUT 'ES_Endpoint/<indexname>/_settings'
{
  "index" : {
    "number_of_replicas" : 1
  }
}
```

## Conclusion
This tutorial will help you to reallocate the unassigned shards in your ES Cluster and avoid any data loss.
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.