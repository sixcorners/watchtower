By default, watchtower will monitor all containers running within the Docker daemon to which it is pointed (in most cases this
will be the local Docker daemon, but you can override it with the `--host` option described in the next section). However, you
can restrict watchtower to monitoring a subset of the running containers by specifying the container names as arguments when
launching watchtower.

```bash
$ docker run -d \
    --name watchtower \
    -v /var/run/docker.sock:/var/run/docker.sock \
    containrrr/watchtower \
    nginx redis
```

In the example above, watchtower will only monitor the containers named "nginx" and "redis" for updates -- all of the other
running containers will be ignored. If you do not want watchtower to run as a daemon you can pass the `--run-once` flag and remove
the watchtower container after its execution.

```bash
$ docker run --rm \
    -v /var/run/docker.sock:/var/run/docker.sock \
    containrrr/watchtower \
    --run-once \
    nginx redis
```

In the example above, watchtower will execute an upgrade attempt on the containers named "nginx" and "redis". Using this mode will enable debugging output showing all actions performed, as usage is intended for interactive users. Once the attempt is completed, the container will exit and remove itself due to the `--rm` flag.

When no arguments are specified, watchtower will monitor all running containers.

## Help
Shows documentation about the supported flags.

```
            Argument: --help
Environment Variable: N/A
                Type: N/A
             Default: N/A
```

## Time Zone
Sets the time zone to be used by WatchTower's logs and the optional Cron scheduling argument (--schedule). If this environment variable is not set, Watchtower will use the default time zone: UTC.
To find out the right value, see [this list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones), find your location and use the value in _TZ Database Name_, e.g _Europe/Rome_.

```
            Argument: N/A
Environment Variable: TZ
                Type: String
             Default: "UTC"
```

## Cleanup
Removes old images after updating. When this flag is specified, watchtower will remove the old image after restarting a container with a new image. Use this option to prevent the accumulation of orphaned images on your system as containers are updated.

```
            Argument: --cleanup
Environment Variable: WATCHTOWER_CLEANUP
                Type: Boolean
             Default: false
```

## Remove attached volumes
Removes attached volumes after updating. When this flag is specified, watchtower will remove all attached volumes from the container before restarting with a new image. Use this option to force new volumes to be populated as containers are updated.

```
            Argument: --remove-volumes
Environment Variable: WATCHTOWER_REMOVE_VOLUMES
                Type: Boolean
             Default: false
```

## Debug
Enable debug mode with verbose logging.

```
            Argument: --debug
Environment Variable: WATCHTOWER_DEBUG
                Type: Boolean
             Default: false
```

## Docker host
Docker daemon socket to connect to. Can be pointed at a remote Docker host by specifying a TCP endpoint as "tcp://hostname:port".

```
            Argument: --host, -h
Environment Variable: DOCKER_HOST
                Type: String
             Default: "unix:///var/run/docker.sock"
```

## Docker API version
The API version to use by the Docker client for connecting to the Docker daemon. The minimum supported version is 1.24.

```
            Argument: --api-version, -a
Environment Variable: DOCKER_API_VERSION
                Type: String
             Default: "1.24"
```

## Include stopped
Will also include created and exited containers.

```
            Argument: --include-stopped
Environment Variable: WATCHTOWER_INCLUDE_STOPPED
                Type: Boolean
             Default: false
```   

## Revive stopped
Start any stopped containers that have had their image updated. This argument is only usable with the `--include-stopped` argument.

```
            Argument: --revive-stopped
Environment Variable: WATCHTOWER_REVIVE_STOPPED
                Type: Boolean
             Default: false
```   

## Poll interval
Poll interval (in seconds). This value controls how frequently watchtower will poll for new images. Either `--schedule` or a poll interval could be defined, but not both.

```
            Argument: --interval, -i
Environment Variable: WATCHTOWER_POLL_INTERVAL
                Type: Integer
             Default: 300
```   

## Filter by enable label
Update containers that have a `com.centurylinklabs.watchtower.enable` label set to true.

```
            Argument: --label-enable
Environment Variable: WATCHTOWER_LABEL_ENABLE
                Type: Boolean
             Default: false
```   

## Without updating containers
Will only monitor for new images, not update the containers.

```
            Argument: --monitor-only
Environment Variable: WATCHTOWER_MONITOR_ONLY
                Type: Boolean
             Default: false
``` 

## Without restarting containers
Do not restart containers after updating. This option can be useful when the start of the containers
is managed by an external system such as systemd.
```
            Argument: --no-restart
Environment Variable: WATCHTOWER_NO_RESTART
                Type: Boolean
             Default: false
```

## Without pulling new images
Do not pull new images. When this flag is specified, watchtower will not attempt to pull
new images from the registry. Instead it will only monitor the local image cache for changes.
Use this option if you are building new images directly on the Docker host without pushing
them to a registry.

```
            Argument: --no-pull
Environment Variable: WATCHTOWER_NO_PULL
                Type: Boolean
             Default: false
``` 

## Without sending a startup message
Do not send a send a message after watchtower started. Otherwise there will be an info-level notification. 

```
            Argument: --no-startup-message
Environment Variable: WATCHTOWER_NO_STARTUP_MESSAGE
                Type: Boolean
             Default: false
```

## Run once
Run an update attempt against a container name list one time immediately and exit.

```
            Argument: --run-once
Environment Variable: WATCHTOWER_RUN_ONCE
                Type: Boolean
             Default: false
``` 

## Scheduling
[Cron expression](https://godoc.org/github.com/robfig/cron#hdr-CRON_Expression_Format) in 6 fields (rather than the traditional 5) which defines when and how often to check for new images. Either `--interval` or the schedule expression could be defined, but not both. An example: `--schedule "0 0 4 * * *"`

```
            Argument: --schedule, -s
Environment Variable: WATCHTOWER_SCHEDULE
                Type: String
             Default: -
``` 

## Wait until timeout
Timeout before the container is forcefully stopped. When set, this option will change the default (`10s`) wait time to the given value. An example: `--stop-timeout 30s` will set the timeout to 30 seconds.

```
            Argument: --stop-timeout
Environment Variable: WATCHTOWER_TIMEOUT
                Type: Duration
             Default: 10s
``` 

## TLS Verification
Use TLS when connecting to the Docker socket and verify the server's certificate. See below for options used to configure notifications.

```
            Argument: --tlsverify
Environment Variable: DOCKER_TLS_VERIFY
                Type: Boolean
             Default: false
```
