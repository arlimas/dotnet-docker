# dotnet-docker

Everything is based on the [docker getting started with dotnet](https://docs.docker.com/language/dotnet/develop/)

[Create the volume ](https://docs.docker.com/engine/reference/commandline/volume_create/)

``` 
docker volume create postgres-data
```

[Create the network](https://docs.docker.com/engine/reference/commandline/network_create/)

``` 
docker network create postgres-net
```

Run the database container

```
docker run --rm -d -v postgres-data:/var/lib/postgresql/data \
  --network postgres-net \
  --name db \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=<your password> \
  postgres
```

Run Adminer container to be able to use admin tools

```
docker run \
  --rm -d \
  --network postgres-net \
  --name db-admin \
  -p 8080:8080 \
  adminer
```

The appplicationSettings ConnectionString where Host=db correspond to the database container

```
"ConnectionStrings": {
    "SchoolContext": "Host=db;Database=my_db;Username=postgres;Password=<your password>"
}
```

Run the application

```
docker run \
  --rm -d \
  --network postgres-net \
  --name dotnet-app \
  -p 5000:80 \
  dotnet-docker
```
Add an `.env` file with a variable defining your database password 

```
POSTGRES_PASSWORD=<your password>
```

Take a look at the docker-compose.yml file!
