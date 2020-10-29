# Docker

## What is Docker?

**What is a container?** a standardized unit of software, a package of code and required runtime. The same container always yields the same application and execution behaviour. Support for containers is built in and available to all modern OS. Docker is a tool system that simplifies the creation and management of containers.

**Why we want standardized and independent "application packages" in software** We often have different development and production environments. Containers allow us to specify versions, runtimes and configurations and ths create/recreate an exact environment that allows developers to work with the same versions as production and remove a lot of potential pitfalls. Also allowing consistency between teams too, it allows the entire team to have the exact same setup as they would encounter on a colleagues machine, or as they would encounter on pre-prod or prod.

**What if you're a solo developer** again, Docker can be helpful here too in that it is possible you will be working on different projects at different times and updating package globally on your system for one project may impact another unrelated project that now has errors that have to be identified and resolved.

## VMs Vs Docker?

At the essence the same result can be achieved with containers and VMs. A VM, however, is like a full virtualised computer within your computer, or host system, so with multiple projects and therefore multiple VMs there quickly becomes a wasted space and resources overhead. With a VM or multiple VM setup we have a lot of duplication of OS resources, code and consumed space. Performance can be highly degraded and this leads to unexpected performance perceptions.

The benefits of containers and Docker over a VM are:

- low impact on OS, fast and use minimal disk space
- sharing, rebuilding and distribution is easier
- Encapsulates environments and apps for the needs, nothing more.

## Images & Containers

We run containers that are based on images. As we have said, an image is the software package including the application and the runtime. A container is a runnable instance of that image.
Images are the blueprint, containers are the instance of our packages running.

we can use precooked images such as the many official packages. For demonstration purposes we will take the official node package. running `docker run node` will begin the download but we need to understand:

- interactive shells are contained, containers are contained.
- to expose the interactive shell of our node image we can use `docker run -it node` where the `-it` instructs that the interactive shell is exposed to us.

## Running an image

Assuming we have an application we wish to `dockerise` and that it is a webapp where the node server is listening on port 80. This does not mean that our container being run needs to be on port 80. We can use the `docker run -p {localport}:{internalport} imageId` notation.

eg. docker run -p 3000:80 8f428c65c994 will run the app on port3000 even though the internal port was set to 80.

## Image layering

We encounter our first potential for Dockerfile optimisation when we talk about image layering. Basically each command in a dockerfile is treated like a layer that builds up the image. While we can advantage from cached layers if we rebuild the exact same image and see the build take a fraction of a second, where anything changes a full rebuild and new image is created.

Where we have a change in one layer (say, source code) all subsequent layers are re-run afterwards because Docker doesn't do any deep analysis. It notices that in the steps of commands one has a difference to the previous build and therefore all further steps must be treated as if there is no history and re-run. In our file we do the copy of source before the `npm install` so any source code changes and rebuild will also run the `npm install` which is inefficient. To circumevent that we can do the following

```dockerfile
# node base
FROM node

# sets the workdir
WORKDIR /app

# copy only the package.json, if no libs are added it doesn't change
COPY package.json /app

# npm install sequence
RUN npm install

# now copy the source code (this is where the mst common change is)
COPY . /app

# expose ports
EXPOSE 80

# set the post sequence commands.
CMD ["node", "server.js" ]
```
