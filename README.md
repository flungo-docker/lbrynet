# Docker image for LBRY.io

Docker image for the [LBRY.io](https://lbry.io) daemon and client. The image uses the latest release binaries that were available at the time of being built.

At present this has just been created to test out the LBRY service and may have bugs or problems as well as lack in documentation, feel free to submit PRs on [GitHub](https://github.com/flungo-docker/lbry.io). Use at your own discretion. Ensure that you make `/home/lbry/.lbryum` persistent if you don't want to loose the LBC in your wallet.

## Starting the daemon

The daemon can be run using the following commands:

```
docker run -d --name lbrynet -p 127.0.0.1:5279:5279 \
  -v $(readlink -f ~/.lbrynet):/home/lbry/.lbrynet \
  -v $(readlink -f ~/.lbryum):/home/lbry/.lbryum \
  -v $(readlink -f ~/Downloads/lbry):/home/lbry/Downloads) \
  flungo/lbry.io
```

The command will use your user's `~/.lbrynet` and `~/.lbryum` directories for persistence and expose the API endpoint locally at `http://localhost:5279/lbryapi`.

If you don't want to mount `/home/lbry/Downloads`, `docker cp` can be used instead to move data in and out of the container.

### Updating

Providing the `/home/lbry/.lbrynet` and `/home/lbry/.lbryum` are persisted, the Docker image can be deleted and started again to get the latest update.

```
docker stop lbrynet
docker rm lbrynet
docker pull flungo/lbry.io
```

If the image is out of date (a newer version has been released than the date this image was last built), send me ([@flungo](https://lbry.slack.com/messages/@flungo/)) a message on the [lbry.io slack](https://slack.lbry.io). The version of the image can also be checked using `docker run --rm flungo/lbry.io lbrynet-daemon --version` (make sure you pull to get the latest). You may also wish to build the image yourself by downloading from [GitHub](https://github.com/flungo-docker/lbry.io) and running `docker build -t flungo/lbry.io src/` inside the repo.

## Using the client

The image contains both `curl` and `lbrynet-cli` commands which can be used through `docker exec`. Examples:

```
docker exec lbrynet \
  curl 'http://localhost:5279/lbryapi' \
  --data '{"method":"resolve_name","params":{"name":"what"}}'
```

```
docker exec lbrynet \
  lbrynet-cli resolve_name name=what
```

If the client is used exclusively through `docker exec` there is no need publish the `5279` port on the host machine.

## Security

The `lbrynet-daemon` is configured to accept conncections from all hosts, so it is down to the user to ensure the security of the container. By default, the host machine and all other containers in the default bridge network will be able to access the LBRY API.
