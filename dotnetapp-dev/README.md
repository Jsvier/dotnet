# .NET Core Development Sample

This .NET Core Docker sample demonstrates how to use Docker in your .NET Core development process. It includes multiple projects and unit testing. The sample works with both Linux and Windows containers.

The [sample Dockerfile](Dockerfile) creates an .NET Core application Docker image based off of the [.NET Core Runtime Docker base image](https://hub.docker.com/r/microsoft/dotnet/).

It uses the [Docker multi-stage build feature](https://github.com/dotnet/announcements/issues/18) to build the sample in a container based on the larger [.NET Core SDK Docker base image](https://hub.docker.com/r/microsoft/dotnet/). It builds and tests the samples and then copies the final build result into a Docker image based on the smaller [.NET Core Docker Runtime base image](https://hub.docker.com/r/microsoft/dotnet/).

This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker). You need the latest Windows 10 or Windows Server 2016 to use [Windows containers](http://aka.ms/windowscontainers). The instructions assume you have the [Git](https://git-scm.com/downloads) client installed.

## Getting the sample

The easiest way to get the sample is by cloning the samples repository with git, using the following instructions.

```console
git clone https://github.com/dotnet/dotnet-docker-samples/
```

You can also [download the repository as a zip](https://github.com/dotnet/dotnet-docker-samples/archive/master.zip).

## Build and run the sample with Docker

You can build and run the sample in Docker using the following commands. The instructions assume that you are in the root of the repository.

```console
cd dotnetapp-dev
docker build -t dotnetapp-dev .
docker run --rm dotnetapp-dev Hello .NET Core from Docker
```

Note: The instructions above work for both Linux and Windows containers. The .NET Core docker images use [multi-arch tags](https://github.com/dotnet/announcements/issues/14), which abstract away different operating system choices for most use-cases.

## Build and run sample for unit testing with Docker

The sample runs unit tests as part of `docker build`. That's useful as a means of getting feedback during `build` (the build will fail), but there isn't an easy way to get the test logs. The sample exposes a test stage that you can build and then run explicity. Using [volume mounting](https://docs.docker.com/engine/admin/volumes/volumes/), you can cause unit tests logs to be written to your local disk for viewing outside of a container.

You can build and run the sample in Docker using the following commands. The instructions assume that you are in the root of the repository. They also assume a location for the sample.

```console
cd dotnetapp-dev
docker build --target test -t dotnetapp-dev:test .
```

You can run the sample on **Windows** using Windows containers using the following command.

```console
docker run -ti -v C:\git\dotnet-docker-samples\dotnetapp-dev\TestResults:C:\app\bottests\TestResults app:test
```

You can run the sample on **Windows** using Linux containers using the following command.

```console
docker run -ti -v C:\git\dotnet-docker-samples\dotnetapp-dev\TestResults:/app/bottests/TestResults app:test
```

You can run the sample on **macOS** or **Linux** using the following command.

```console
docker run -ti -v ./TestResults:/app/bottests/TestResults app:test
```

Note: The instructions above work for both Linux and Windows containers. The .NET Core docker images use [multi-arch tags](https://github.com/dotnet/announcements/issues/14), which abstract away different operating system choices for most use-cases.


## Build and run the sample locally

You can build and run the sample locally with the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) using the following instructions. The instructions assume that you are in the root of the repository.

```console
cd dotnetapp-dev
dotnet run Hello .NET Core
```

You can produce an application that is ready to deploy to production locally using the following command.

```console
dotnet publish -c release -o out
```

You can run the application on **Windows** using the following command.

```console
dotnet out\dotnetapp.dll
```

You can run the application on **Linux or macOS** using the following command.

```console
dotnet out/dotnetapp.dll
```

Note: The `-c release` argument builds the application in release mode (the default is debug mode). See the [dotnet run reference](https://docs.microsoft.com/dotnet/core/tools/dotnet-run) for more information on commandline parameters.

## Docker Images used in this sample

The following Docker images are used in this sample

* [microsoft/dotnet:2.0-sdk](https://hub.docker.com/r/microsoft/dotnet)

## Related Resources

* [.NET Core Docker samples](../README.md)
* [.NET Framework Docker samples](https://github.com/Microsoft/dotnet-framework-docker-samples)
