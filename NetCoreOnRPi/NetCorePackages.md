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

- The sdk is not required to run an application. It is just needed if you want to create, compile and publish your application on the device itself.
Since the Raspberry PI is a good prototyping environment, it can be a good idea to install it, but I did not because I prefer simulating a real production environment.

**The Raspberry Pi image is now ready to deploy and running applications using .NET Core 2.0**
To install the SDK or the Runtime proceed as follows:
[https://www.microsoft.com/net/download/linux](https://www.microsoft.com/net/download/linux)

