# Basic Concept 
- image
- container

Container is the running instance of a docker image. You can modify the container once it runs, but this does not change the original image. However, you can commit the change in container to create a new image. 

# Docker Usage

## Install Docker
1. Install docker on Ubuntu follow [official installation guide](https://docs.docker.com/engine/install/ubuntu/)
2. Manage Docker as a non-root user follow the [post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/)
3. To use GPU, install nvidia container toolkit follow [this official tutorial](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker)


## An example command to run docker container, with chatgpt explaination
```
docker run -v /home/chenwang/VCD/softgym:/workspace/softgym -v /home/chenwang/anaconda3:/home/chenwang/anaconda3 -v /tmp/.X11-unix:/tmp/.X11-unix --rm --runtime=nvidia --gpus all -e DISPLAY=$DISPLAY  -e QT_X11_NO_MITSHM=1 -it xingyu/softgym:latest bash
```

### Explaination by chatgpt
Sure! This is a complex command, so let's break it down piece by piece:

1. **`docker run`**: This is the base command to run a Docker container.

2. **`-v /home/chenwang/VCD/softgym:/workspace/softgym`**: This flag mounts the local directory `/home/chenwang/VCD/softgym` into the container at `/workspace/softgym`. Any changes made inside this directory in the container will reflect in the host, and vice versa.

3. **`-v /home/chenwang/anaconda3:/home/chenwang/anaconda3`**: Similarly, this flag mounts the local directory `/home/chenwang/anaconda3` into the container at the same path.

4. **`-v /tmp/.X11-unix:/tmp/.X11-unix`**: This mounts the X11 Unix domain socket from the host into the container, allowing GUI applications to be displayed on the host's X server.

5. **`--rm`**: This flag will automatically remove the container when it exits. This is good for keeping your system clean from stopped containers.

6. **`--runtime=nvidia`**: This sets the runtime to NVIDIA, allowing the container to access the GPU. This is typically used for tasks that require GPU computation.

7. **`--gpus all`**: This flag specifies that all available GPUs on the host should be accessible by the container.

8. **`-e DISPLAY=$DISPLAY`**: This sets the `DISPLAY` environment variable inside the container to the same value as the host. This is essential for running GUI applications inside the container.

9. **`-e QT_X11_NO_MITSHM=1`**: This sets another environment variable that is often used to work around shared memory issues when running Qt applications on X11.

10. **`-it`**: The `-i` flag keeps STDIN open even if not attached, and the `-t` flag allocates a pseudo-TTY. This allows you to interact with the container as if you were in a terminal session with it.

11. **`xingyu/softgym:latest`**: This is the name of the Docker image that you want to run. `latest` refers to the latest version of the image.

12. **`bash`**: This is the command that will be run inside the container. In this case, it starts a bash shell.

So, in a nutshell, this command runs a Docker container using the specified image and parameters, setting up various mounts and environment variables to facilitate GPU computing and running GUI applications, and opens an interactive bash shell inside the container.

> This covers the most usage cases to run a docker container. One additional thing that might be needed is to map port with `-p 8888:8888/tcp`.

## Use `jupyter notebook` inside docker container
First you need to have the port mapped out. Then run
```
jupyter notebook --ip=0.0.0.0 --no-browser --allow-root
```
- `--ip=0.0.0.0` tells jupyter to listen on all available network interfaces inside the container, so it should be accessible from the host.


## use `docker commit` to save the changes in a container to a new image
Here is an answer by chatgpt:

If you've made changes to a running container and want to save those changes to a new image, you can use the `docker commit` command. Here's how to do it:

1. **Start and modify your container**: You've already done this part by running your container and then installing or configuring the necessary components inside it.

2. **Get the Container ID**: Get the ID or name of the container:

   ```bash
   docker ps -a
   ```

   Find your container in the list and make a note of its ID or name.

3. **Commit the changes**:
   
   Commit the changes in the container to a new Docker image. You can specify a new name and optionally a tag for the image:

   ```bash
   docker commit <container_id_or_name> <new_image_name>:<tag>
   ```

   For example:

   ```bash
   docker commit abc123 my-new-image:latest
   ```

4. **Verify the New Image**: Check that the new image is listed among your local images:

   ```bash
   docker images
   ```

   You should see `my-new-image` (or whatever name you chose) in the list.

> `latest` is just the default tag that docker will use when no tag name is specified.

> **Note**: While the `docker commit` approach is quick and straightforward for capturing changes, for more systematic work or production setups, it's a good practice to capture all the changes you made in a `Dockerfile`. This ensures reproducibility and makes it easier for others (or your future self) to understand how the image is constructed.

## Reconnect to a stopped container
If you do not use `-rm` option, when you exit the container this container still exists, but will be in the *stopped* state. You can reuse this container. To restart and reconnect to a stopped container:
```
docker start <container_id_or_name>
docker attach <container_id_or_name>
```

## Get a bash inside a running container
```
docker exec -it <container_id_or_name> /bin/bash
```

## docker container proxy configuration
See [this article](https://neucrack.com/p/286).
```
vim ~/.docker/config.json
```
Add the following content
```json
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://172.17.0.1:8123",
     "httpsProxy": "http://172.17.0.1:8123",
     "noProxy": "localhost,127.0.0.1,.daocloud.io"
   }
 }
}
```
In this way, `build` and `run` will automatically set `http_proxy` and `https_proxy` to the corresponding value. 
> It should be noted that instead of `127.0.0.1`, `172.17.0.1` is used. This is because the docker container and the host does not share the same network environment. `172.17.0.1` is docker's bridge network address. If you use `--network=host`, then you can use `127.0.0.1`. 
