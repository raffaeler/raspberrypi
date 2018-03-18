# Installing the SDK on the Raspberry PI
The SDK was not available on the Raspberry PI before version 2.1

### Note: this article refers to a preview of the 2.1 version

Prepare the presequisites as specified here: [NetCorePackages](NetCorePackages.md)

Download the sdk at the following URL: [https://dotnetcli.blob.core.windows.net/dotnet/Sdk/master/dotnet-sdk-latest-linux-arm.tar.gz](https://dotnetcli.blob.core.windows.net/dotnet/Sdk/master/dotnet-sdk-latest-linux-arm.tar.gz)

```
curl -OSL https://dotnetcli.blob.core.windows.net/dotnet/Sdk/master/dotnet-sdk-latest-linux-arm.tar.gz
```

If you installed a previous release of the runtime or the sdk, this command will tell you the location, which is probably `/opt/dotnet/` with a symbolic link in `/usr/local/bin`.
```
whereis dotnet
```
If a previous installation is found, you can go into that folder and rename or remove the previous installation. In this case the symbolic link is already created and you can omit the directory and symbolic link creation.

Otherwise you can proceed to create the directory, extract the sdk content there and finally creating the symbolic link:
```
sudo mkdir -p /opt/dotnet
sudo tar zxf dotnet-sdk-latest-linux-arm.tar.gz  -C /opt/dotnet
sudo ln -s /opt/dotnet/dotnet /usr/local/bin
```

You can now go back to the home folder with `cd` and the dotnet cli should show the version of the sdk you just installed:
```
pi@piraf6:~ $ dotnet --version
2.1.300-preview2-008375
```

## Test the sdk installation
The best way to see if the installation was successful is to create a new project, build and run it. Please note that the nuget restore process could take some time.

```
pi@piraf6:~ $ mkdir test1
pi@piraf6:~ $ cd test1
pi@piraf6:~/test1 $ dotnet new console
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on /home/pi/test1/test1.csproj...
  Restoring packages for /home/pi/test1/test1.csproj...
  Generating MSBuild file /home/pi/test1/obj/test1.csproj.nuget.g.props.
  Generating MSBuild file /home/pi/test1/obj/test1.csproj.nuget.g.targets.
  Restore completed in 3.71 sec for /home/pi/test1/test1.csproj.

Restore succeeded.

pi@piraf6:~/test1 $ dotnet run
Hello World!
pi@piraf6:~/test1 $

```

## Measuring the time of each step
Let's now repeat the previous test, but measuring the time taken from each step.

```
pi@piraf6:~ $ mkdir test1
pi@piraf6:~ $ cd test1
pi@piraf6:~/test1 $ time dotnet new console --no-restore
The template "Console Application" was created successfully.

real 0m8.149s
user 0m15.720s
sys  0m1.230s
pi@piraf6:~/test1 $ time dotnet build
Microsoft (R) Build Engine version 15.7.66.2115 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restoring packages for /home/pi/test1/test1.csproj...
  Generating MSBuild file /home/pi/test1/obj/test1.csproj.nuget.g.props.
  Generating MSBuild file /home/pi/test1/obj/test1.csproj.nuget.g.targets.
  Restore completed in 3.68 sec for /home/pi/test1/test1.csproj.
  You are working with a preview version of the .NET Core SDK. You can define the SDK version via a global.json file in the current project. More at https://go.microsoft.com/fwlink/?linkid=869452
  test1 -> /home/pi/test1/bin/Debug/netcoreapp2.1/test1.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:01:18.32

real 1m29.714s
user 1m12.190s
sys  0m4.550s
pi@piraf6:~/test1 $ time dotnet bin/Debug/netcoreapp2.1/test1.dll
Hello World!

real 0m0.964s
user 0m0.940s
sys  0m0.040s
pi@piraf6:~/test1 $
```
The build time takes a while, but all the rest is definitely great.
You may want to use a PC with either Windows or Linux to develope the application and deploy the compiled version on the Raspberry PI right after.
To simplify the Continuous Deployment process, take a look to the [https://github.com/raffaeler/deploytool]().


