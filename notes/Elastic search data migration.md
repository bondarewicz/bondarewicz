---
title: Elastic search data migration
created: '2021-09-07T10:17:06.175Z'
modified: '2021-09-07T10:20:37.804Z'
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

### Reindex

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

