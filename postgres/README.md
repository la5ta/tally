To create a dump:

``` bash
# Postgress Dump
docker exec -it tally_postgres_1 /bin/bash
  # Inside the shell...
  pg_dump -U postgres -d postgres > dump.sql
# Copy dump.sql from the container
docker cp tally_postgres_1:/dump.sql ./

# Hasura metadata
hasura init hasura --endpoint https://tally-graphql.com/ # if we dont have any metadata
hasura migrate create "init" --from-server --schema "public"
hasura migrate apply --version "<version>" --skip-execution
hasura migrate status
```