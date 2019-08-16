You can host a "local" RuckZuck Server to cache binaries locally.

The easiest way is by running the ruckzuck Docker container from:
https://cloud.docker.com/u/zanderr/repository/docker/zanderr/ruckzuck

# Installation
Get the container:
```
docker pull zanderr/ruckzuck:latest
```

Start the container:
```
docker run --name ruckzuck -d -e "localURL=http://192.168.2.109:5000" -p 5000:5000/tcp -p 5001:5001/udp zanderr/ruckzuck
```
>Note: the Variable localURL must represent the url to access the container

# Verify
As soon as the container is started, browse to your "localURL" and you will get the same page as on https://ruckzuck.tools
The Server will listen to broadcasts on UDP:5001, so RuckZuck clients within the same subnet should find the "Proxy" automatically... When you start RuckZuck.exe, you will find your "localURL" as REST-API URL:
![RuckZuck URL](https://user-images.githubusercontent.com/11909453/63156270-9c8ee480-c014-11e9-9a0f-09082691c87c.png)

you can also configure your local RuckZuck Server by setting the following registry keys:
```
if((Test-Path -LiteralPath "HKLM:\SOFTWARE\Policies\RuckZuck") -ne $true) {  New-Item "HKLM:\SOFTWARE\Policies\RuckZuck" -force -ea SilentlyContinue };
New-ItemProperty -LiteralPath 'HKLM:\SOFTWARE\Policies\RuckZuck' -Name 'WebService' -Value "http://192.168.2.109:5000" -PropertyType String -Force -ea SilentlyContinue;
New-ItemProperty -LiteralPath 'HKLM:\SOFTWARE\Policies\RuckZuck' -Name 'Broadcast' -Value 0 -PropertyType DWord -Force -ea SilentlyContinue;
```
>Note: Broadcast=0 will disable the initial broadcast to find a RuckZuck Server

# Caching
The first Client who request content from the internet will trigger the server to download the file(s) to **wwwroot/content/{contentid}**. The content URL within the package will then be redirected to your localURL.
The content within the container will be cleaned if you re-publish or update your container. To keep the content files, you can redirect **wwwroot/content** to your physical host.

# known Limitations
* Only packages with valid URLs can be cached, URL's generated from a PowerShell will still be downloaded from the clients.
* Some packages can not be downloaded from the Server. As workaround you can copy the content to **wwwroot/content/{contentid}**
* Download errors ca occur if multiple clients requesting the same file and the file is not yet cached. As workaround, use RuckZuck.exe to pre-download files you need.
* The Server will not re-download content if the signature or file hash has changed. You will have to remove this content from **wwwroot/content/{contentid}**


