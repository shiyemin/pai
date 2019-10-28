## Postgresql

Postgresql is an internal service used for structured information persistence. The service uses the folder `/paiInternal` on the master node for data storage. The real data directory for the postgresql would be `/paiInternal/pgdata`. Default configurations are as follows:

```yaml
postgresql:
  user: root
  passwd: rootpass
  port: 5432
  db: openpai
``` 

One can override these settings by editing `services-configuration.yaml` .

### Table Initialization

If it is the first time to launch the service, it will execute `src/init_table.sql` to initialize the table structure. The initialization won't be fired if the service detects old data. If you want to re-trigger it, please remove `/paiInternal/pgdata` manually and restart the service.

### How to Access the Database

The database connection string is written to the cluster configuration object in `config/postgresql.py`. One can use `cluster_cfg['postgresql']['connectionStr']` to retrieve it in any template files.

Particularly, the connection string is exposed as an environmental variable in `rest-server`:

```bash
# in rest-server pod
SQL_CONNECTION_STR=postgresql://root:rootpass@10.151.40.36:5432/openpai
```
