# Publishing the App

### Create a pre-compiled and self-contained application 

Pre-compiling an application is the process transforming the IL code into native assembly code (in case of the Raspberry PI, ARM7 assembly). This process is also known as `AOT` compilation which means `Ahead Of Time` in contrast to `JIT` which means `Just in Time`. The JIT compilation is the process that compiles the code from `IL` (Intermediate Language) used by .NET into native assembly code at runtime. The JIT compilation happens on a per-executed-method basis, which means that only the code being effectively used will be compiled on the fly.

JIT is a great solution because it is architecture-neutral, allowing an assembly to be shared on different OS and architecture (unless other constraints are met). Anyway, as soon as the application has a long list of dependencies, the initial bootstrap will require a longer amount of time. This happens for example in classic ASP.NET Core MVC application where the infrastructure itself needs to be compiled.

Another reason not to stay with JIT compilation is where the application lifecycle is short and the application is run several times. This is more frequent in applications designed to be run on Linux whose architecture prediliges running multiple processes instead of splitting the functionalities in class libraries.

Whenever you see an unacceptable delay in running the application, you may want to pre-compile the application.

At the time of writing (currently .NET Core 3 is just a pre-release) this process must be done on the device itself and requires the application to be deployed in SCD mode (Self-contained Deployment). This basically means that the build output folder will contain not only the binaries for your application but everything else is required, including third-party libraries and the whole .NET Core files required to run. Using `SCD` the runtime or SDK (eventually) installed as shared won't be used from the application.

The parameter used to obatin AOT compilation is `-p:PublishReadyToRun=true`. The typical command line used to build the application in the `myApp` folder is the following:
```
 dotnet publish -c Release -r linux-arm -p:ReadyToRun=true -o myApp
```
