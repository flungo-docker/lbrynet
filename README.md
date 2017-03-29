# Docker image for LBRY.io

Docker image for the [LBRY.io](https://lbry.io) daemon and client. At present this has just been created to test out the LBRY service and may have bugs or problems as well as lack in documentation. Use at your own discretion.

## Starting the daemon

The daemon can be run using the following commands:

```
docker run -d --name lbrynet-daemon -p 127.0.0.1:5279:5279 \
  -v $(readlink -f ~/.lbryum):/home/lbry/.lbryum \
  -v $(readlink -f ~/.lbrynet):/home/lbry/.lbrynet \
  flungo/lbry.io
```

The command will use your user's `~/.lbrynet` and `~/.lbryum` directories for persistence and expose the API endpoint locally at `http://localhost:5279/lbryapi`.

To access downloads, an additional volume can be attached to `/home/lbry/Downloads` or `docker cp lbrynet-daemon:<path> .` can be used.

## Using the client

Details to be added.
