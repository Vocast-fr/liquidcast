# liquidcast

Relay a radio stream using Liquidsoap + Icecast in a Docker stack 

## Config

```
cp .env_example .env
nano .env
```


| Name                      | Purpose                                                       |
|---------------------------|---------------------------------------------------------------|
| `STREAM_HTTP_URL`  | HTTP URL of a radio stream.                            |
| `ICECAST_MOUNT_MP3`  | Icecast mountpoint name. Should end with '.mp3'     |
| `ICECAST_PORT`  | Port number available on host to listen relayed stream / output icecast on liquidsoap (port 8000). |

Replace these two environment values with the ones you want


## Start

You should install [Docker Compose](https://docs.docker.com/compose/install/) before. 
Once installed, build and run the app with Compose


```
docker-compose up -d
```

If the terminal returns something like

```
Starting icecast_1    ... done
Starting liquidsoap_1 ... done
```

You can now listen the stream on your localpoint : `localhost:${ICECAST_PORT}/${ICECAST_MOUNT_MP3}`


## Go further : use the stack for advanced features, with Liquidsoap script file 

Example with `./yourscript.liq` 

```

stream = input.http(getenv("STREAM_HTTP_URL"))

## write what you want stream = ...

output.icecast(
     %mp3,
     host = getenv("ICECAST_HOST"),
     port = 8000,
     password = getenv("ICECAST_SOURCE_PASSWORD"), 
     mount = getenv("ICECAST_MOUNT_MP3"), 
     mksafe(stream)
)
```


1- `docker-compose.yml` : Add under liquidsoap --> `volumes:` 

```
./yourscript.liq:/liquidsoap/yourscript.liq
```

2- `docker-compose.yml` : Change the `liquidsoap` command 

```
command: liquidsoap -v --debug /liquidsoap/yourscript.liq
```
