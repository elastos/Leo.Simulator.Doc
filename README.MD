
# Documents

A docker image is offered for running the rendezvous service in local machine instead of the free server /dns4/ws-star.discovery.libp2p.io/tcp/443/wss/p2p-websocket-star
```bash
docker pull libp2p/websocket-star-rendezvous:release
docker run -d -p 9090:9090 --name rendezvous libp2p/websocket-star-rendezvous:release
```
BTW, To disable prometheus metrics run the server with -e DISABLE_METRICS=1
