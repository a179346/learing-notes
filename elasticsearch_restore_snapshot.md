# Elasticsearch restore snapshot

[AWS Elasticsearch Documentation](https://docs.aws.amazon.com/zh_tw/elasticsearch-service/latest/developerguide/es-managedomains-snapshots.html)


- [Elasticsearch restore snapshot](#elasticsearch-restore-snapshot)
- [Commands](#commands)
  - [Get all snapshot repositories](#get-all-snapshot-repositories)
  - [Get all snapshots](#get-all-snapshots)
  - [Restore a snapshot](#restore-a-snapshot)
  - [Delete Index](#delete-index)

# Commands

## Get all snapshot repositories

> curl -XGET '***[elasticsearch-endpoint]***/_snapshot?pretty'

##  Get all snapshots

> curl -XGET '***[elasticsearch-endpoint]***/_snapshot/***[repository-name]***/_all?pretty'

## Restore a snapshot

> curl -XPOST '***[elasticsearch-endpoint]***/_snapshot/***[repository-name]***/***[snapshot-name]***/_restore'

## Delete Index

> curl -XDELETE '***[elasticsearch-endpoint]***/***[index-name]***'