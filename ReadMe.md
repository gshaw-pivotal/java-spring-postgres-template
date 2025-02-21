## To start docker image

From root: `docker compose up -d`

## To stop docker image

From root: `docker compose down`

## To access Postgres within the docker image / container

Access the command line within the container (via a terminal):
`docker exec -it backend-db bash`

Log into Postgres DB:
`psql -U postgres -d backend-db`

List tables in the DB/schema:
`\dt`