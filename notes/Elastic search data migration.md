---
title: Elastic search data migration
created: '2021-09-07T10:17:06.175Z'
modified: '2021-09-07T14:29:54.772Z'
---

# Elastic search data migration

### White list old cluster

`PUT https://new-cluster-url/_cluster/settings`

```
{
  "persistent" : {
    "reindex.remote.whitelist" : "old-cluster-url"
  }
}
```

### Reindex sync

`POST https://new-cluster-url/_reindex`

```
{
  "source": {
    "remote": {
      "host": "https://old-cluster-url",
      "username": "_REDACTED_",
      "password": "_REDACTED_"
    },
    "index": "some-index-*"
  },
  "dest": {
    "index": "some-index-reindex-v1"
  }
}
```

### Reindex async

request 

`POST https://new-cluster-url/_reindex?wait_for_completion=false`

```
{
  "source": {
    "remote": {
      "host": "https://old-cluster-url",
      "username": "_REDACTED_",
      "password": "_REDACTED_"
      "socket_timeout": "5m",
      "connect_timeout": "90s"
    },
    "index": "some-index-*"
    "query": {
      "match_all": {}
    }
  },
  "dest": {
    "index": "some-index-reindex-v1"
  }
}
```

response

```
{
    "task": "nOcLKa7LRba0-d9mFGaENw:200367"
}
```

### Get task details

`GET https://new-cluster-url/_tasks/nOcLKa7LRba0-d9mFGaENw:200367`



