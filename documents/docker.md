# Docker

**What is a container?** a standardized unit of software, a package of code and required runtime. The same container always yields the same application and execution behaviour. Support for containers is built in and available to all modern OS. Docker is a tool system that simplifies the creation and management of containers.

**Why we want standardized and independent "application packages" in software** We often have different development and production environments. Containers allow us to specify versions, runtimes and configurations and ths create/recreate an exact environment that allows developers to work with the same versions as production and remove a lot of potential pitfalls. Also allowing consistency between teams too, it allows the entire team to have the exact same setup as they would encounter on a colleagues machine, or as they would encounter on pre-prod or prod.

**What if you're a solo developer** aagain, Docker can be helpful here too in that it is possible you will be working on different projects at different times and updating package globally on your system for one project may impact another unrelated project that now has errors that have to be identified and resolved.
