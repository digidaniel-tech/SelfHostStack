# Mono

This is a docker image for running .NET Framework application using mono.

Docker hub:
[digidanieltech/mono](https://hub.docker.com/r/digidanieltech/mono)

## What is Mono?

Mono is a free OSS framework that was built to make it possible to run
.NET framework application on Linux, this framework was built before the time
of .NET Core that is now the main framwork used to build multi platform 
applications.

## Why Mono and not .NET Core

While .NET Core is replacing Mono there is still some applications out there
that is still built on .NET Framework and has not been or will ne be migrated
to .NET Core but still some people is being reliable on, there fore this
project is still alive and also why I setup this image.

## How to use

Let's say you have an application named MyApp.exe, then you will create an
Dockerfile with similar content as below, where using this image you copying
in the executable to the container and then run it using the mono service.

```Dockerfile
FROM digidanieltech/mono:latest
RUN mkdir /opt/app
COPY MyApp.exe /opt/app/MyApp.exe
CMD ["mono", "MyApp.exe"]
```

This can then be run from an terminal of choice by running the two commands
below.

```Shell
docker build -t myapp .
docker run -it --rm myapp
```
