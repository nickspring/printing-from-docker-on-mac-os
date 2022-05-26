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

