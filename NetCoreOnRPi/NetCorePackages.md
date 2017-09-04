# Start developing with .NET Core and the Raspberry PI
## Preparing the Raspberry Pi for NetCore

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

- The sdk is **not available** (at the time of writing). It is just needed if you want to create, compile and publish your application on the device itself and it is normally used only on a development or prototype environment.

**The Raspberry Pi image is now ready to deploy and running applications using .NET Core 2.0**

## Installing the runtime
The runtime is not needed but it can simplify the development lifecycle because it will allow to deploy far less files every time you modify something in your application.

The NetCore 2.0 runtime archive for ARM-Linux is the following:
[https://dotnetcli.blob.core.windows.net/dotnet/Runtime/2.0.0/dotnet-runtime-2.0.0-linux-arm.tar.gz](https://dotnetcli.blob.core.windows.net/dotnet/Runtime/2.0.0/dotnet-runtime-2.0.0-linux-arm.tar.gz)

You may transfer the file using Bitvise or WinSCP. Otherwise you can download it directly from the Raspberry PI using the following command:
```
curl -sSL -o dotnet-runtime-2.0.0-linux-arm.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/Runtime/2.0.0/dotnet-runtime-2.0.0-linux-arm.tar.gz
```
The installation consists in creating a folder, unpacking the content of this file and finally add a symbolic link to make the dotnet CLI available:
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

## Running an application
In order to test the environment, I will use in this example a very simple application called "vanilla", a console application that just print some system information.
There are few alternatives, just pick what is best for you:
1. clone the repo, compile it on Windows/Linux and then deploy the binaries to be used with the runtime
2. download the pre-compiled binaries from my repo [https://github.com/raffaeler/NetCore2App/releases/](https://github.com/raffaeler/NetCore2App/releases/)
3. as by point 2, but deploy the complete set of binaries (SCD) to be used regardless the runtime being installed or not.

### Case 1: Clone the repo and compile the app
The app **must** be compiled on a PC supporting the SDK, therefore this operation cannot be done on the Raspberry PI since there is still no SDK available.
The repo is the following: [https://github.com/raffaeler/NetCore2App](https://github.com/raffaeler/NetCore2App)

From the command line of a machine with the SDK, clone the repo, compile and run the app to test it locally
```
git clone https://github.com/raffaeler/NetCore2App.git
cd VanillaSolution/vanilla
dotnet run
```
Now create the binaries to be copied to the Raspberry PI:
Open a console and enter the project folder.
1. Small deployment, requiring the runtime installed:
```
dotnet publish -c Release -r linux-arm --self-contained=false
```
2. Large deployment, totally self-contained
The command retrieves the nuget dependencies, compiles the project in release mode and publishes the application and all the dependencies (including NetCore 2 itself) inside the folder `vanilla\bin\Release\netcoreapp2.0\linux-arm`.
```
dotnet publish -c Release -r linux-arm --self-contained
```

### Case 2: download the pre-compiled binaries
This will work only if you installed the Net Core 2 runtime.
```
curl -sSL -o vanilla.linux-arm.runtime.tar https://github.com/raffaeler/NetCore2App/releases/download/v1.0.0/vanilla.linux-arm.runtime.tar
mkdir vanilla2
tar xf vanilla.linux-arm.runtime.tar -C vanilla2
cd vanilla2/
```
run the app using the CLI:
```
$ dotnet vanilla.dll
This is a very basic application to verify that dotnet is installed correctly
Vanilla v1.0 by @raffaeler, 2017
Machine:     piraf6
OS         : Linux
Description: Linux 4.9.41-v7+ #1023 SMP Tue Aug 8 16:00:15 BST 2017
$
```

### Case 3: download the SCD
The SCD for vanilla can be downloaded directly from the device and extracted in a folder with these commands:
```
curl -sSL -o vanilla.linux-arm.scd.tar https://github.com/raffaeler/NetCore2App/releases/download/v1.0.0/vanilla.linux-arm.scd.tar
mkdir vanilla3
tar xf vanilla.linux-arm.scd.tar -C vanilla3
cd vanilla3/
```
The linux executable has no extension, it is just called 'vanilla' and before it can be executed, you need to give it the eXecutable attribute:
```
chmod +x vanilla
```
Finally, the vanilla console app can be executed:
```
$ ./vanilla
This is a very basic application to verify that dotnet is installed correctly
Vanilla v1.0 by @raffaeler, 2017
Machine:     piraf6
OS         : Linux
Description: Linux 4.9.41-v7+ #1023 SMP Tue Aug 8 16:00:15 BST 2017
$
```


