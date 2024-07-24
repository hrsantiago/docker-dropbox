Container for generating a Docker container for running Dropbox.

References:

https://github.com/otherguy/docker-dropbox

# Build all images


```
docker build --no-cache -t dropbox .
```

# Tag and push it

```
docker tag dropbox wingeon/dropbox
docker push wingeon/dropbox
```

# Running

```
docker network create dropbox

docker run --log-opt max-size=10m --log-opt max-file=10 \
	-d -it --restart=always --name=dropbox_mydrop --net="dropbox" \
	-e "TZ=$(readlink /etc/localtime | sed 's#^.*/zoneinfo/##')" \
	-e "DROPBOX_UID=$(id -u)" -e "DROPBOX_GID=$(id -g)" -e "POLLING_INTERVAL=20" \
	-v "~/dropboxes/mydrop:/opt/dropbox/Dropbox" dropbox

docker logs dropbox_mydrop
```

Open the link provided from the logs of the container and authorize it.

# Useful commands

```
docker exec -ti dropbox_mydrop gosu dropbox dropbox status
docker exec -ti dropbox_mydrop gosu dropbox dropbox exclude add Dropbox/some_folder
```
