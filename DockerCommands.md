
## General Commands
First up, here are some basics to get you started:

```bash docker version ``` – Displays detailed information about your Docker CLI and daemon versions.

```bash docker system info ``` – Lists data about your Docker environment, including active plugins and the number of containers and images on your system.

```bash docker help ``` – View the help index, a reference of all the supported commands.

```bash docker <command> --help ``` – View the help information about a particular command, including detailed information on the supported option flags.

## Build Images
These commands relate to building new images from Dockerfiles:

```bash docker build . ``` – Build the Dockerfile in your working directory into a new image.

```bash docker build -t example-image:latest . ``` – Build the Dockerfile in your working directory and tag the resulting image as example-image:latest.

```bash docker build -f docker/app-dockerfile ``` – Build the Dockerfile at the docker/app-dockerfile path.

```bash docker build --build-arg foo=bar . ```– Build an image and set the foo build argument to the value bar.

```bash docker build --pull . ``` – Instructs Docker to pull updated versions of the images referenced in FROM instructions in your Dockerfile, before building your new image.

```bash docker build --quiet . ``` – Build an image without emitting any output during the build. The image ID will still be emitted to the terminal when the build completes.

## Run Containers
After building an image, use these commands to run containers:

```bash docker run example-image:latest ``` – Run a new container using the example-image:latest image. The output from the container’s foreground process will be shown in your terminal.

```bash >docker run example-image:latest demo-command ``` – Supplying an argument after the image name sets the command to run inside the container; it will be appended to the image’s entrypoint. (It’s possible to override the entrypoint with the docker run command’s --entrypoint flag.)

```bash docker run --rm example-image:latest ``` – The --rm flag instructs Docker to automatically remove the container when it exits instead of allowing it to remain as a stopped container.

```bash docker run -d example-image:latest ```– Detaches your terminal from the running container, leaving the container in the background.

```bash docker run -it example-image:latest ``` – Attaches your terminal’s input stream and a TTY to the container. Use this command to run interactive commands inside the container.

```bash docker run --name my-container example-image:latest ``` – Names the new container my-container.

```bash docker run --hostname my-container example-image:latest ```– Set the container’s hostname to a specific value (it defaults to the container’s name).

```bash docker run --env foo=bar example-image:latest ```– Set the value of the foo environment variable inside the container to bar.

```bash docker run --env-file config.env example-image:latest ``` – Populate environment variables inside the container from the file config.env. The file should contain key-value pairs in the format foo=bar.

```bash docker run -p 8080:80 example-image:latest ``` – Bind port 8080 on your Docker host to port 80 inside the container. It allows you to visit localhost:8080 to access the network service listening on port 80 inside the container.

```bash docker run -v /host-directory:/container-directory example-image:latest``` – Bind mount /host-directory on your host to /container-directory inside the container. The directory’s contents will be visible on both sides of the mount.

```bash docker run -v data:/data example-image:latest``` – Mount the named Docker volume called data to /data inside the container.

```bash docker run --network my-network example-image:latest ```– Connect the new container to the Docker network called my-network.

```bash docker run --restart unless-stopped example-image:latest ``` – Set the container to start automatically when the Docker daemon starts, unless the container has been manually stopped. Other restart policies are also supported.

```bash docker run --privileged example-image:latest ```– Run the container with privileged access to the host system. This should usually be disabled to maintain security.

## Manage Containers
After you’ve started some containers, you can use the following set of commands to manage them:

```bash docker ps ``` – List all the containers currently running on your host. (Learn more: How to use docker ps command)

```bash docker ps -a ```– List every container on your host, including stopped ones.

```bash docker attach <container>``` – Attach your terminal to the foreground process of the container with the ID or name <container>.

```bash docker commit <container> new-image:latest ``` – Save the current state of <container> to a new image called new-image:latest.

```bash docker inspect <container> ``` – Obtain all the information Docker holds about a container, in JSON format.

```bash docker kill <container> ``` – Send a SIGKILL signal to the foreground process running in a container, to force it to stop.

```bash docker rename <container> my-container ``` – Rename a specified container to my-container.

```bash docker pause <container> and docker unpause <container> ``` – Pause and unpause the processes running within a specific container.

```bash docker stop <container>``` – Stop a running container.

```bash docker start <container>``` – Start a previously stopped container.

```bash docker rm <container>``` – Delete a container by its ID or name. Use the -f (force) flag to delete a container that’s currently running.

Read more: How to Stop and Remove Docker Containers.

## Copy to and From Containers
The ```bash docker cp ``` command facilitates bi-directional copying between containers and your host machine:

```bash docker cp example.txt my-container:/data ``` – Copy example.txt from your host to /data inside the my-container container.

```bash docker cp my-container:/data/example.txt /demo/example.txt ``` – Copy `/data/example.txt` out of the my-container container, to `/demo/example.txt` on your host.

If you need to move files or folders between two containers, you should copy from the first container to your host, then onwards into the second container.

Execute Commands in Containers
The ```bash docker exec ``` command allows you to run a new process inside a currently running container:

```bash docker exec my-container demo-command ``` – Run demo-command inside my-container; the process’ output will be shown in your terminal

```bash docker exec -it my-container demo-command``` – Run a command interactively by attaching your terminal’s input stream and a pseudo-TTY.

Access Container Logs
```bash docker logs <container>``` – This command streams the existing log output from a container into your terminal window, then exits.

```bash docker logs <container> --follow``` – This variation emits all existing logs, then continues to stream new logs into your terminal as they’re stored.

```bash docker logs <container> -n 10``` – Get the last 10 logs from a container.

Logs are collated from the standard output and error streams emitted by the container’s foreground process.


## Manage Images
The following commands interact with images stored on your Docker host:

```bash docker images``` – List all stored images.

```bash docker rmi <image>``` – Delete an image by its ID or tag. Deletion of images which have multiple tags must be forced using the -f flag.

```bash docker tag <image>``` example-image:latest – Add a new tag (example-image:latest) to an existing image (<image>).

## Pull and Push Images
```bash docker push example.com/user/image:latest``` – Push an image from your Docker host to a remote registry. The image is identified by its tag, which must reference the registry you’re pushing to.

```bash docker pull example.com/user/image:latest``` – Manually pull an image from a remote registry to make it available on your host.

When the image’s tag omits a registry URL, the Docker Hub registry will be used as the default.

## Manage Networks
These commands administer the Docker networks on your host:

```bash docker create network my-network ```– Create a new network called my-network; it will default to using the bridge driver.

```bash docker create network my-network -d host ```– Use the -d flag to select an alternative driver, such as host.

```bash docker network connect <network> <container> ```– Connect a container to an existing network.

```bash docker network disconnect <network> <container> ```– Remove a container from a network it’s currently connected to.

```bash docker network ls ```– List all the Docker networks available on your host, including built-in networks such as bridge and host.

```bash docker network rm <network> ```– Delete a network by its ID or name. This is only possible when there are no containers currently connected to the network.

## Manage Volumes
The following commands relate to the management of storage volumes:

```bash docker volume create my-volume ``` – Create a new named volume called my-volume.

```bash docker volume ls``` – List the volumes present on your host.

```bash docker volume rm``` – Delete a volume, which will destroy the data within it. The volume must not be used by any container.

### Use Configuration Contexts
Configuration contexts allow you to connect to multiple Docker daemon instances from a single installation of the Docker CLI.

```bash docker context create my-context --host=tcp://host:2376,ca=~/ca-file,cert=~/cert-file,key=~/key-file ```– Create a new context called my-context to connect to a specified Docker host.

```bash docker context update <context> ```– Modify the configuration of a named context; the command accepts the same arguments as docker context create.

```bash docker context ls ```– List the contexts available in your Docker config file.

```bash docker context use <context> ```– Switch to a named context. Subsequent docker commands will be executed against the Docker host configured in the newly selected context.

```bash docker context rm <context> ```– Delete a context by its name.

## Create SBOMs
Docker now has integrated SBOM generation capabilities. SBOMs are indexes of the packages included in your container images.

```bash docker sbom example-image:latest ```– Produce an SBOM for the image tagged example-image:latest. The SBOM will be shown in your terminal.

```bash docker sbom example-image:latest --output sbom.txt ```– Produce an SBOM and save it to sbom.txt.

```bash docker sbom example-image:latest --format spdx-json ```– Produce an SBOM in a standard machine-parseable format, such as SPDX (spdx-json), CycloneDX (cyclonedx-json), or Syft JSON (syft-json).

## Scan for Vulnerabilities
Docker also has a built-in image vulnerability scanner that’s powered by Snyk:

```bash docker scan example-image:latest ```– Scan for vulnerabilities in the image tagged example-image:latest. The results will be shown in your terminal.

```bash docker scan example-image:latest --file Dockerfile ```– The `--file` argument supplies the path to the Dockerfile that was used to build the image. When the Dockerfile is available, more detailed vulnerability information is produced.

```bash docker scan example-image:latest --severity high``` – Only report vulnerabilities that are high severity or higher. The --severity flag also supports low and medium values.

## Docker Hub Account
These commands interact with your Docker Hub account:

```bash docker login ```– Login to your account. You’ll be prompted to supply credentials interactively. You must login before you can push images. Logging in also helps you avoid hitting public pull rate limits.

```bash docker logout ```– Logs you out of your account.

```bash docker search nginx``` – Searches Docker Hub for images matching the supplied search term (nginx, in this example).

## Clean Up Unused Resources
It’s normal for a regularly used Docker installation to accumulate a large number of resources, many of which become redundant as you create replacements. These commands will clean up your environment:

```bash docker system prune ``` – Removes unused data, including dangling image layers (images with no tags).

```bash docker system prune -a ```– Extends the prune process by deleting all unused images, instead of only dangling ones.

```bash docker system prune --volumes``` – Includes volume data in the prune process. This will delete any volumes that aren’t used by a container.

```bash docker image prune``` – Removes dangling images, without affecting any other types of data.

```bash docker image prune -a```– Removes all unused images.

```bash docker network prune``` – Removes unused networks.

```bash docker volume prune ```– Removes unused volumes.

```bash docker system df``` – Reports your Docker installation’s total disk usage.

The prune commands will prompt you to confirm your intentions before any resources are deleted. You can disable the prompt by setting the -f (force) flag.