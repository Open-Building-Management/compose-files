# compose files to use with docker-compose

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
