## Docker/Docker-Compose Migrations
This is a crash course in docker/docker-compose migrations for onboarding Application Engineers at Sourcegraph. This course assumes that you already have a single-container, local docker deployment of Sourcegraph, and a docker-compose (no users yet) deployment using GCP. 

### Setup 

If you already have local and GCP instances of sourcegraph set up, you can move to the next step. If you do not have both of these already set up, please *visit the following pages in the Sourcegraph documentation and do so before starting this crash course*.

1. Instructions for [deploying a local docker instance](https://docs.sourcegraph.com/admin/install/docker)
2. Instructions for [deploying a docker-compose instance in GCP](https://docs.sourcegraph.com/admin/install/docker-compose/google_cloud)

Once your local instance is deployed, create an account, then add a codehost and a few small repos to your instance so there is something to migrate when the time comes. *NOTE: Once you have deployed your docker-compose instance on GCP, do not register a new user yet. That information will be added automatically when we migrate the database dumps later*.

### Migrating

1. First, find your local instance's `CONTAINER_ID` by running your local instance, then executing the command `docker ps`. You will see the following output:
```
> docker ps
CONTAINER ID        IMAGE
...                 sourcegraph/server
```
Take the value from the `CONTAINER_ID` column and export it as a variable in your terminal:
```
export CONTAINER_ID=<value from the previous command>
```
2. Generate database dumps from `codeintel-db` and `psql` and save them to the /tmp folder. For `codeintel-db`:
```
docker exec -it "$CONTAINER_ID" sh -c 'pg_dump -C --username=postgres sourcegraph-codeintel' > /tmp/codeintel_db.out
```
and for the postgres database:
```
docker exec -it "$CONTAINER_ID" sh -c 'pg_dump -C --username=postgres sourcegraph' > /tmp/sourcegraph_db.out
```

