# How to get Wireguard Private Key from a NordVPN Connection

1 - In your docker server, run the following command:
```
docker run -it --name bruch-nord --cap-add=NET_ADMIN --sysctl net.ipv6.conf.all.disable_ipv6=0 thiagobruch/nordvpn_wg_key
```
2 - The command above will download the image and start the container. Once the container is running, run the command below:
```
nordvpn login
```
