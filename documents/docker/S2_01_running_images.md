# Docker

## Images & Containers

We run containers that are based on images. As we have said, an image is the software package including the application and the runtime. A container is a runnable instance of that image.
Images are the blueprint, containers are the instance of our packages running.

we can use precooked images such as the many official packages. For demonstration purposes we will take the official node package. running `docker run node` will begin the download but we need to understand:

- interactive shells are contained, containers are contained.
- to expose the interactive shell of our node image we can use `docker run -it node` where the `-it` instructs that the interactive shell is exposed to us.

## Running an image

Assuming we have an application we wish to `dockerise` and that it is a webapp where the node server is listening on port 80. This does not mean that our container being run needs to be on port 80. We can use the `docker run -p {localport}:{internalport} imageId` notation.

eg. docker run -p 3000:80 8f428c65c994 will run the app on port3000 even though the internal port was set to 80.

## Stopping & Restarting containers

Stopping, we use `docker stop {container name}`

using `docker run` creates a new container. When we have a stopped a container and nothing about our application or configuration has changed then there is no need to generate a new container. We can restart the existing one.

To restart we use `docker start container_name`, however this will not block a terminal like the `docker run` command. You can verify its running with the `docker ps` command.

This highlights two modes:

- attached mode (running in foreground, default for `docker run`)
- detached mode (running in background, default for `docker start`)

we can set the modes or change the modes with flags.

- If we want to use the `docker run` which has a default of attached in a detached mode we add the `-d` flag.
- On starting a detached run it will show the id. You can then attach yourself to a running container with `docker attach {container_name}`.
- For containers being restarted which has a default of unattached and you wish to run in attached mode we can use the `-a` flag to start in attached mode.
- if we want to see the logs of a container we run `docker logs {container_name}` and this will show us the output from a container we have been detached from. This is a snapshot though and if we want to follow that in attached mode, we would run with a `-f` flag to follow, which again would block the terminal.

Because we don't want to leave containers we don't need running, stopping and removing is a strategy we must look at. It's possible for us to be able to remove the container once it is complete.

Assuming we have a container running a webapp that we want to expose port 80 on a local port of 3000 in detached mode and to automatically remove on completion we can use:
`docker run -p 3000:80 --rm containerId`

In this mode we will see our webapp running via `docker ps` but if we choose to close it and run `docker ps -a` we will no longer see it, it will be gone. Why is this useful? Well the typical usecase here is that you would only be stopping your web service when you had to rebuild the code anyway so it would be replaced by a new container.
