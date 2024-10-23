# How to get WireGuard Private Key from a NordVPN Connection in order to use with Gluetun Container

<B>1 - </b>In your docker server, run the following command:
```
docker run --rm -it --name nordvpn_wg_key --cap-add=NET_ADMIN --sysctl net.ipv6.conf.all.disable_ipv6=0 thiagobruch/nordvpn_wg_key
```
<B>2 - </B>The command above will download the image and start the container. Once the container is running, run the command below:
```
nordvpn login
```
<B>3 - </B>The command above will display a URL. Copy the URL, paste in your browser and login.<br>
An example of the URL is: https://api.nordvpn.com/v1/users/oauth/login-redirect?attempt=ffd3f93d-6aa4-f231-e422-f7cdd348f93d
<BR><BR>
<B>4 - </B>After you logged in, you will see a button showing "Continue". DO NOT CLICK.<BR>
Instead, right click on the Continue Button and select "Copy Link Address"
<BR><BR>
<B>5 - </B>Use the URL that you copy in the item above and past int he command below replacing the "nordvpn://..." as the example below:
```
nordvpn login --callback "nordvpn://login?action=login&exchange_token=3YhVhkY5Tm2o4gYn2weRsfw0RUP9V4ee8bGz3zGQYiuynb6idkUaHZsG0xkTFCA77XSHkeig8utbrNh7yU7Fv6%3D%3D&status=done"
```
You will receive a message similar to the one below:<BR>
Welcome to NordVPN! You can now connect to VPN by using 'nordvpn connect'.
<BR><BR>
<B>6 - </B>Run the following command to connect:
```
nordvpn connect
```
You will receive a message similar to the one below:<BR>
You are connected to United States #5941 (us5941.nordvpn.com)!<BR>
<BR>
<B>7 - </B>Run the following command:
```
wg showconf nordlynx|grep PrivateKey
```
An example of the response would be:
PrivateKey = 0skE58AkMhLQ2vctnzB4VKykcVonVOQLbt1Swzyvbif=

<B>8 - </B>Now you can use the PrivateKey to connect directly using WireGuard

# Gluetun Container for NordVPN with WireGuard

1 - Create a directory (i.e. /docker/gluetun)
2 - Create a file named docker-compose.yml and edit using your favorite editor (i.e. nano, vi)
3 - Use the code below:
```
version: "3"
services:
  gluetun:
    image: qmcgaw/gluetun
    container_name: gluetun_nordvpn
    cap_add:
      - NET_ADMIN
    ports: #make sure to include this port to be used as your proxy port
      - 8890:8888
    environment:
      - VPN_SERVICE_PROVIDER=nordvpn
      - VPN_TYPE=wireguard
      - WIREGUARD_PRIVATE_KEY=bER4QZNdOvQzxx9nuQ01UCz1wdx6DzUcAXZu93n5K0Y=
      - SERVER_COUNTRIES=Canada
      - HTTPPROXY=on
      - HTTPPROXY_LISTENING_ADDRESS=:8888
    volumes:
      - /docker/gluetun/data:/gluetun
    restart: unless-stopped
```
4 - Save and run the container using the command below:
```docker compose up -d
```

