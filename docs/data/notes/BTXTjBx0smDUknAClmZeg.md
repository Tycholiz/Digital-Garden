
Pgloader can operate in 2 ways:
1. Load data from files (ex. CSV)
2. Migrate the whole db to Postgres (ex. Mysql to Postgres)

Easiest way is to use the docker container:
```
docker run --rm --name pgloader dimitri/pgloader:latest pgloader SOURCE TARGET
```

# Resources
[Docs](https://pgloader.readthedocs.io/en/latest/)
