# The docker hub development workflow
Ensure to have completed the steps as for [DockerOnPi.md](DockerOnPi.md).
## Create a docker hub account
This is not a required step, but it makes extremely easier to test the platform because it will allow to publish an image from a PC and retrieving it from the Raspberry PI.

Simply go to the [https://hub.docker.com/](https://hub.docker.com/) and create a free account. The most relevant restriction to free accounts is that anything you publish on the hub will have public visibility.
In order to use the newly created account from the command line, we have to instruct the CLI about the username and password. **Please be aware that the following step will give the current user the permission to access to the docker hub account.**
Repeat the following operation from both Windows and the Rasperry PI.
```
docker login
```
and type username and password. [See the official documentation here](https://docs.docker.com/engine/reference/commandline/login/).
Should you change idea and want to remove the association from the current user from the docker account, type the following command:
```
docker logout
```
[See the official documentation here](https://docs.docker.com/engine/reference/commandline/logout/).

## Prepare and publish an application
At this point, you can start running containers on the device. Of course you will need to only use **ARM images**. It is obvious, but otherwise it can be hard to figure out why it is failing.
The Microsoft official tutorial for preparing a container can be found here: [https://github.com/dotnet/dotnet-docker-samples](https://github.com/dotnet/dotnet-docker-samples)
Anyway, it is extremely easy to dockerize any application. Here there are the basic steps:
1. Create a new application (console or Asp.net core) using NetCore 2.0
2. Add a text file called ".dockerignore" with the following content:
```
bin/
obj/
```
3. Add a text file called "dockerfile.arm32" to the project with the following content:
```
FROM microsoft/dotnet:2.0-sdk AS build-env
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# build runtime image
FROM microsoft/dotnet:2.0.0-runtime-stretch-arm32v7
WORKDIR /app
COPY --from=build-env /app/out ./
ENTRYPOINT ["dotnet", "led2.dll"]
```
4. In the "dockerfile.arm32" replace "led2.dll" with the binary produced by your app ("app.dll" or whatelse).
5. Open the console (cmd or powershell) and create the container:
```
docker build -t youraccount/led2 -f dockerfile.arm32 .
```
6. Push the container to the docker hub. Please note that, unless you have a subscription and instruct to make a private publishing, the container will be publicly visible to anyone.
```
docker push youraccount/led2
```

## Run the container on the device
Now let's go back to the Raspberry PI.
The following command will pull, if needed, the image from the docker hub, run it and finally clean-up the container state.
```
docker run --rm youraccount/led2 9
```
- run : specifies to run the container
- --rm: specifies to clean-up the state of the container. [See the official documentation here](https://docs.docker.com/engine/reference/run/#clean-up-rm).
- youraccount/led2: the relative address of your image on the docker hub
- 9: a command line parameter specific of the application

If you prefer, you can first pull the image from your account and then run it.
This is the command to pull the image:
```
docker pull youraccount/led2
```
After this, you can run the app in the container using the same command. The container will not need to be downloaded anymore until it gets updated.
```
docker run --rm youraccount/led2 9
```

**Important note:** the first time you install any image with NetCore 2 the download will be longer. Thanks to the docker image layering system, any other application requiring the same base image will be downloaded lightning faster.
