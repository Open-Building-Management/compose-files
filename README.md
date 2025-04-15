
<details><summary><h1>install docker (+compose) & portainer if needed</h1></summary>

https://docs.docker.com/engine/install/debian/ or https://docs.docker.com/engine/install/ubuntu/

it should install all the stack (docker and compose) but for more info on docker compose : https://github.com/docker/compose

check docker version :
```
docker --version
docker compose version
```

manage log size : `nano /etc/docker/daemon.json`

with the following content :

```
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "3m",
    "max-file": "3",
    "labels": "production_status",
    "env": "os,customer"
  }
}
```
and restart the docker daemon : `sudo systemctl restart docker`

for container management, it is nice to use portainer :

- create a `portainer_data` volume `sudo docker volume create portainer_data`
- run portainer as a daemon : `sudo docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:lts`

you can reach portainer UI on https://127.0.0.1:9443

cf https://docs.portainer.io/start/install-ce

</details>

# run emoncms alone

For persistent datas, create a folder named `data` at the root of the host.
If you just want an ephemeral test without persistent datas, remove the `/data` volume in the compose file.

Adjust `MYSQL_USER` and `MYSQL_PASSWORD` to the custom values of your taste. 
**Please note you can only do that at the creation of the mariadb database.**

For the MQTT broker, you can change the credentials values when you want
To use an external broker, adjust `MQTT_HOST` to your needs but the port has to be 1883.

To customize ssl :
1) mount a folder with your customized credentials
2) modify the env vars `CRT_FILE` and `KEY_FILE` with the path to the customized credentials

Then : `sudo docker compose up -d`

# emoncms and emonhub together

download a configuration file for emonhub :

```
wget https://raw.githubusercontent.com/openenergymonitor/emonhub/master/conf/emonhub.conf
```
fill the emoncms credentials in the `[[MQTT]]` section and run the compose files :

```
sudo docker compose -f compose.yaml -f compose_emonhub.yaml up -d
```
to stop :
```
sudo docker compose -f compose.yaml -f compose_emonhub.yaml down
```

You can of course monitor the log with portainer.
