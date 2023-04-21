# CUPS-SULDR-docker

Run a CUPS print server on a remote machine to share USB printers over WiFi. 
Includes the proprietary samsung driver(driver2-1.00.39) from [https://www.bchemnet.com/suldr/](https://www.bchemnet.com/suldr/)

## Build
```sh
docker build -t cups .
```
## Usage
```sh
docker run -d --name cups \
    --restart unless-stopped \
    -p 631:631 \
    --privileged \
    -e CUPSADMIN=batman \
    -e CUPSPASSWORD=batcave_password \
    -e TZ="Europe/Budapest" \
    -v /var/run/dbus:/var/run/dbus \
    -v /dev/bus/usb:/dev/bus/usb \
    -v <persistent-config-folder>:/etc/cups \
    localhost/cups:latest
```

#### Optional parameters
- `name` -> whatever you want to call your docker image. using `cups` in the example above.
- `volume` -> adds a persistent volume for CUPS config files if you need to migrate or start a new container with the same settings

Environment variables that can be changed to suit your needs, use the `-e` tag
| # | Parameter    | Default            | Type   | Description                       |
| - | ------------ | ------------------ | ------ | --------------------------------- |
| 1 | TZ           | "America/New_York" | string | Time zone of your server          |
| 2 | CUPSADMIN    | admin              | string | Name of the admin user for server |
| 3 | CUPSPASSWORD | password           | string | Password for server admin         |

## Server Administration
You should now be able to access CUPS admin server using the IP address of your headless computer/server http://192.168.xxx.xxx:631, or whatever. If your server has avahi-daemon/mdns running you can use the hostname, http://printer.local:631. (IP and hostname will vary, these are just examples)

If you are running this on your PC, i.e. not on a headless server, you should be able to log in on http://localhost:631

## Thanks
Based on the work done by **anujdatar**: [https://github.com/anujdatar/cups-docker](https://github.com/anujdatar/cups-docker)
