You can host a "local" RuckZuck Server to cache binaries locally.

The easiest way is by running the ruckzuck Docker container from:
https://cloud.docker.com/u/zanderr/repository/docker/zanderr/ruckzuck

# Installation
Get the container:
```
docker pull zanderr/ruckzuck
```

Start the container:
```
docker run --name ruckzuck -d -e "localURL=http://192.168.2.109:5000" -p 5000:5000/tcp -p 5001:5001/udp zanderr/ruckzuck
```
>Note: the Variable localURL must represent the url to access the container

# Verify
As soon as the container is started, browse to your "localURL" and you will get the same page as on https://ruckzuck.tools
The Server will listen to broadcasts on UDP:5001, so RuckZuck clients within the same subnet should find the "Proxy" automatically... 


