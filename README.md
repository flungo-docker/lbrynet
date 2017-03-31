# Docker image for LBRY.io

Docker image for the [LBRY.io](https://lbry.io) daemon and client.

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
```

## Using the client

The image contains both `curl` and `lbrynet-cli` commands which can be used through `docker exec`. Examples:

```
docker exec lbrynet curl 'http://localhost:5279/lbryapi' --data '{"method":"resolve_name","params":{"name":"what"}}'
```

```
docker exec lbrynet lbrynet-cli resolve_name name=what
```

If the client is used exclusively through `docker exec` there is no need publish the `5279` port on the host machine.

## Security

The `lbrynet-daemon` is configured to accept conncections from all hosts, so it is down to the user to ensure the security of the container. By default, the host machine and all other containers in the default bridge network will be able to access the LBRY API.
