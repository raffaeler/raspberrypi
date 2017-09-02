# Start developing with .NET Core and the Raspberry PI
## Preparing the Raspberry Pi for NetCore without installing the SDK or the Runtime

### Minimal packages required to run NetCore 2

Just be sure to use the "Stretch" version of a Debian derived distribution.

1. Open a terminal with sudo privileges
sudo su

2. Install the following packages. The list was taken from the documentation [here](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x) or the NetCore repository [here](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md).
- libunwind8
- libunwind8-dev
- gettext
- libicu-dev
- liblttng-ust-dev
- libcurl4-openssl-dev
- libssl-dev
- uuid-dev
- unzip

You can install them with this single command:
```
apt-get install libunwind8 libunwind8-dev gettext libicu-dev liblttng-ust-dev libcurl4-openssl-dev libssl-dev uuid-dev unzip
```
The documentation and the repo include the instructions to install the Runtime and the SDK.

- The runtime is not required if you are going to publish the application as a "Self Contained Deployment" (SCD). It is instead required if you want to rely on a shared runtime for many applications. The decision is just yours. The advantage of the SCD is to be able to run different versions of your application and CoreCLR runtime side-by-side which can be precious in a production environment.

- The sdk is not available (at the time of writing). It is just needed if you want to create, compile and publish your application on the device itself and it is normally used only on a development or prototype environment.

**The Raspberry Pi image is now ready to deploy and running applications using .NET Core 2.0**
To install the SDK or the Runtime proceed as follows:
[https://www.microsoft.com/net/download/linux](https://www.microsoft.com/net/download/linux)

The NetCore 2.0 runtime image for ARM-Linux is the following:
https://dotnetcli.blob.core.windows.net/dotnet/Runtime/2.0.0/dotnet-runtime-2.0.0-linux-arm.tar.gz](https://dotnetcli.blob.core.windows.net/dotnet/Runtime/2.0.0/dotnet-runtime-2.0.0-linux-arm.tar.gz)

You may transfer the file using Bitvise or WinSCP. Otherwise you can download it directly from the Raspberry PI using the following command:
```
curl -sSL -o dotnet-runtime-2.0.0-linux-arm.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/Runtime/2.0.0/dotnet-runtime-2.0.0-linux-arm.tar.gz
```
The installation consist in creating a folder, unpacking the content of this file and finally add a symbolic link to make the dotnet CLI available:
```
sudo su
mkdir -p /opt/dotnet
tar zxf dotnet-runtime-2.0.0-linux-arm.tar.gz -C /opt/dotnet
ln -s /opt/dotnet/dotnet /usr/local/bin
exit
```
At this point you can invoke the dotnet CLI from any terminal window:
```
dotnet
```
