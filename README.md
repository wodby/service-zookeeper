# ZooKeeper service for Kubernetes on Wodby

Run ZooKeeper as a data store for Kubernetes applications managed by Wodby.

This repository defines the Wodby service manifests and operational
configuration for ZooKeeper.

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Wodby stacks using this service

- [Drupal application stack](https://github.com/wodby/stack-drupal)
- [Solr application stack](https://github.com/wodby/stack-solr)
- [ZooKeeper application stack](https://github.com/wodby/stack-zookeeper)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `zookeeper` |
| Type | Datastore |
| Versions | `3.9` by default |
| Workloads | `main` (StatefulSet), primary |
| Containers | `zookeeper` using `wodby/zookeeper` |
| Endpoints | `zookeeper`: TCP 2181 |
| Volumes | Data, 5 GB |
| Helm | chart `oci://registry-1.docker.io/wodby/zookeeper`; version `0.1.0` |

## Use this service

Use this service through [Drupal application stack](https://github.com/wodby/stack-drupal), [Solr application stack](https://github.com/wodby/stack-solr),
[ZooKeeper application stack](https://github.com/wodby/stack-zookeeper), or reference `zookeeper` from a custom Wodby stack.

A service is a reusable component and does not deploy by itself. The stack
defines its links, settings, versions, resources, and relationship to the rest
of the application.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Keep service, workload, container, endpoint, link, volume, config, and
derivative names stable unless dependent stacks and app-level overrides are
updated at the same time.

Validate the manifests with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
