# Access to printers from the Docker on Mac OS

To access to the printer from the docker container on Mac OS needs some tricks. 

## Step 1. CUPSD.CONF changing

On you Mac OS please open /etc/cups/cupsd.conf and change:

```Listen localhost:631```
to
```Listen 0.0.0.0:631```

after this line also add:

```BrowseAddress @LOCAL```

and in the end of section ```<Location />``` add:

```Allow @LOCAL```

Please save this file and restart CUPS daemon:

```sudo launchctl stop org.cups.cupsd && sudo launchctl start org.cups.cupsd```

## Step 2. Network sharing for needed printers

Please go to Settings -> Printers & Scanners and for each printer, which you will use from the Docker, and make sure the option "Share this printer on the network" is enabled.

## Step 3. Starting docker container

```
ip=$(ifconfig | grep -o "inet [^\n]*" | grep -v 127.0.0.1 | head -1 | cut -d\  -f2)

docker run -it --rm \
-e CUPS_SERVER=$ip:631 \
--privileged \
--network=host \
--entrypoint bash \
IMAGE_ID
```

REMARK: there is entrypoint to the bash just for testing only. You could change it according to your needs.

## Step 4. Testing

This command (in the container) should show your printers.

```lpstat -p```
