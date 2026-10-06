# ZooKeeper on Wodby

What Wodby sets up for this ZooKeeper service. In Wodby stacks it is the coordination store of SolrCloud; applications normally do not talk to it directly.

## How services reach it

- Host: the name of this app service inside the environment. Client port: `2181`.
- No client authentication or TLS is configured and no token is generated. Access is limited to the environment's internal network.
- A linked Solr service uses the host as its `ZK_HOST` and protects its own znodes with digest credentials.

## Generated configuration

On every start the container renders `zoo.cfg` in ZooKeeper's `conf` directory from environment variables and appends the server list. With one replica it runs standalone. Never edit the file; set variables on the service instead:

| Variable | Setting | Default |
| --- | --- | --- |
| `ZOO_TICK_TIME`, `ZOO_INIT_LIMIT`, `ZOO_SYNC_LIMIT` | `tickTime`, `initLimit`, `syncLimit` | `2000`, `5`, `2` |
| `ZOO_MAX_CLIENT_CNXNS` | `maxClientCnxns` | `60` |
| `ZOO_AUTOPURGE_PURGE_INTERVAL`, `ZOO_AUTOPURGE_SNAP_RETAIN_COUNT` | snapshot auto-purge | `0` (off), `3` |
| `ZOO_4LW_COMMANDS_WHITELIST` | allowed four-letter commands | `stat,ruok,conf,isro` |

## Data

- Snapshots are in `/data`; the manifest sets `ZOO_DATA_LOG_DIR` so the transaction log is in `/data/datalog`, on the same volume.
- The `data` volume is optional. Without it the data is lost when the container is replaced. For SolrCloud that means the collections' configuration, config sets and security settings, which live in ZooKeeper and not on Solr's volume.
- The manifest declares no backups, imports or actions.

## Check the result

From this service's container:

- `echo ruok | nc -w 2 localhost 2181` answers `imok`.
- `make stat -f /usr/local/bin/actions.mk` prints the server mode and connected clients.
