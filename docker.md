# Docker
- https://docs.docker.com/get-started/

-- Twitter @slace / 2017 https://www.youtube.com/watch?v=i7yoXqlg48M


- virtual machines vs containers
  - container runs natively on Linux and shares the kernel of the host machine with other containers. It runs a discrete process, taking no more memory than any other executable, making it lightweight.
  - a virtual machine (VM) runs a full-blown “guest” operating system with virtual access to host resources through a hypervisor. In general, VMs provide an environment with more resources than most applications need.

- docker provides
  - a/many linux container(s)
    - user space isolation
    - process isolation
    - the ability to run an application with no system dependencies
- image
  - OOP equivalent: a class definition
  - what you start from
  - what you produce as an output
  - an executable package that includes everything needed to run an application
    - the code
    - a runtime
    - libraries
    - environment variables
    - configuration files
- container
  - OOP equivalent: the implementation of the class  
  - a runtime instance of an image
    - what the image becomes in memory when executed
      - i.e. an image with state, or a user process
- host machine
  - the machine which is running docker

- interacting with docker
  - commandline
    `docker command options arguments`




## docker references
- https://linuxconfig.org/a-hands-on-introduction-to-docker-containers
- https://www.toptal.com/devops/getting-started-with-docker-simplifying-devops
- https://hackaday.com/2018/09/05/intro-to-docker-why-and-how-to-use-containers-on-any-system/
- https://www.edureka.co/blog/docker-tutorial
- https://www.linode.com/docs/applications/containers/introduction-to-docker/
- https://docker-curriculum.com/
- https://medium.freecodecamp.org/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b
- https://www.youtube.com/watch?v=V9IJj4MzZBc
