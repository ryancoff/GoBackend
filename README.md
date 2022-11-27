# GoBackend


# Docker Quick-start
## List docker images
```
docker images | grep -i postgres
```

## Run a new container
```
docker run --name postgres12 -e POSTGRES_USER=root -e POSTGRES_PASSWORD=secret -p 5432:5432 -d postgres:12-alpine
```

## Resume the container
```
docker ps -a | grep -i postgre 
```

## E.g: Container ID - 91dc0cbb41a9
```
docker start 91dc0cbb41a9
```

## Check current status & config (port,tag,..)
```
docker ps
```

## Access console of running container
```
docker exec -it postgres12 psql -U root
select now();
\q
```

## Check log of running container


One thing you might notice here is: Postgres doesn’t ask for password, although we’ve set it when running the container. It’s because by default, the Postgres container sets up a trust authentication locally, so password is not required when connecting from localhost (inside the container).


# TablePlus setting up connection

* Now we enter the name of the connection. I’m gonna call it postgres12.
* The host is localhost (or 127.0.0.1), and the port is 5432 by default
* The username is root, and the password is secret, as we configured when running the postgres container.
* The default database name is root, same as the username, since we didn’t explicitly config it when starting the container.

## You might need to drop (old) database `simple_bank` before import new one.

```
docker exec -it postgres12 psql -U root

SELECT 
    pg_terminate_backend(pid) 
FROM 
    pg_stat_activity 
WHERE 
    -- don't kill my own connection!
    pid <> pg_backend_pid()
    -- don't kill the connections to other databases
    AND datname = 'simple_bank'
    ;

DROP DATABASE simple_bank;

\q
```