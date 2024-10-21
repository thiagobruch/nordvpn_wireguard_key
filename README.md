# How to get Wireguard Private Key from a NordVPN Connection

1 - In your docker server, run the following command:
```
docker run -it --name bruch-nord --cap-add=NET_ADMIN --sysctl net.ipv6.conf.all.disable_ipv6=0 thiagobruch/nordvpn_wg_key
```
2 - The command above will download the image and start the container. Once the container is running, run the command below:
```
nordvpn login
```
3 - The command above will display a URL. Copy the URL, paste in your browser and login.<br>
An example of the URL is: https://api.nordvpn.com/v1/users/oauth/login-redirect?attempt=ffd3f93d-6aa4-f231-e422-f7cdd348f93d
<BR><BR>
4 - After you logged in, you will see a button showing "Continue". DO NOT CLICK.<BR>
Instead, right click on the Continue Button and select "Copy Link Address"
<BR><BR>
5 - 
