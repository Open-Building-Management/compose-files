# compose files to use with docker-compose

choose a folder and modify the dockerfile with your parameters

to customize ssl : 
1) mount a folder with your customized credentials
2) modify the env vars CRT_FILE and KEY_FILE with the path to the customized credentials

then : `sudo docker compose up -d`
