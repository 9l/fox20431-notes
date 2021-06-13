# Docker

## CLI

Build the iamge.
```sh
docker build -t <tag_name> directory
```

Start a new container.
```sh
docker run -dp 3000:3000 getting-started
```

List images/containers/process status.
```sh
docker image ls
docker container ls
docker ps
```
Note that you can add parameter `-a` to show all.

Stop the container.
```sh
docker stop <the-container-id>
```
Note that you can get `Container ID` from `docker ps` command.

Start the container.
```sh
docker start <the-container-id>
```
Note that it's different from `docker run` command. `docker run` will create the new container, but `docker start` start the stopped container.

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE.
```sh
docker tag getting-started YOUR-USER-NAME/getting-started
```

Share the application.
```sh
docker push YOUR-USER-NAME/getting-started
```
Note that the tag is the same as the username.