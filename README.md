# SelfHostStack

## Description

Collection of self-hosted service configs, base images (Mono, Alpine, etc.),
and DevOps tools. Includes Docker Compose, Terraform,
CI/CD pipelines (Jenkins, GitLab), and more. Perfect for managing and
experimenting with modern infrastructure setups.

## Structure

### docker-images

In the docker images folder I store all images separated by responsibility

Most of the images is built with Alpine Linux as it's own base image to keep
the image size as small as possible.

All images used always has an version number to it, I never use latest to 
prevent unexpected problems, when using these services I always recommend to
specify a version even if the example uses `:latest` I never recommend to use
it in production.

#### Base

The base directory holds all docker images build with the sole purpose to be
used as base images for other images, like an php-fpm image for services that 
expects to be run on top of php-fpm.

Under this directory each base image is separated depending on what it's main 
purpose is for instance if it is for php-fpm then the folder is named php-fpm,
beneath each base image directory there is multiple versions for each base
service so they can be updated independently, a structure can look like this:

```shell
...
|- base
|  |- php-fpm
|  |  |- 8.4.3_alpine3.21.2
|  |  |- 8.3.16_alpine3.21.2
...
```

#### Services

The service directory hold an docker image for each service, most services
is bult on top of an base image from the base directory, so if looking into
a specific service and you want to know what more it contains from the base
image then you will be able to find the base image inside the previous base
directory.

Each service os separated on specific service it run and beneath that it is
separated on version of the service, it can look something like this:

```shell
...
|- services
|  |- magento2-ce
|  |  |- 2.4.7-p3_php-fpm8.4.3
|  |  |- 2.4.7-p3_php-fpm8.3.16
...
```
and so on...

### Stacks

Stacks are compose files that spins up an complete setup of an service using
the docker image files from the docker-image directory.

There can be multiple services or one service depending on what is required
to get the specific service to run, for instance an Magento 2 needs to setup 
an database, php-fpm and nginx to work. While Sonarr only needs mono to run.

Each stack I try to upgrade so it is always using the latest image of each
service but because I don't run all the stacks privately I might not have 
had time to update to the latest version, if that happens then please leave
an PR or add an issue and I will try to update it asap.

The selection of services in each stack is from recommendations for the service
maintainers, so event if the stack contains a specific database but can be run
with another database then you can change to the selection of your choice.

It is always a good idea to look over the stack before running it to see of the
versions is out-dated or there is another service that better fits your need.

## FAQ

### Why re-invent the wheel when there is already official docker images?

Some of you might asking why I do this when there already is a lot of official
images that can be used? There is multiple reasons but the main one is because
IT IS FUN! I love writing dockerfiles and can do it all day every day.

Another reason is that I then know exactly what is run on the image, there is 
always a security consern to use an official image and by building my own I 
know what the image contain and doesn't contain.

And the last bigger reason is that by building my own docker images I get to 
learn how each service works under the hood, what makes them run and how are
they configured, so it is a huge learning oppertunity for me.

