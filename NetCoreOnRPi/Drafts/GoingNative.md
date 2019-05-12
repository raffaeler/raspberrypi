# Going Native on the Raspberry PI

**Early Draft**


Before or later, it will come the moment to compile some C++ library for ARM.

In simple cases C# and PInvoke may avoid the need to create a C++ library. But as soon as the amount of PInvokes gets larger, you will discover the need to put all the native code there and create a C# wrapper instead. Also, there are already many good C++ libraries that are fast, efficient and provides all the functionalities you may need.

When using .NET Core, remember that, starting with C# versions 7.1, 7.2 and 7.3, there are new tools and APIs that allows to speed up memory management. I am talking about `Memory<T>`, `Span<T>`, `MemoryMarshal` and related language features allowing to map/cast an arbitrary portion of memory, even if native, to a .NET type.

A part from this, the goal of this document is to provide the best possible solution to compile, in a timely manner, some C++ sources for the Raspbian distribution commonly used on the Raspberry PI.
The provided suggestion is valid for any distribution and is up to you to decide which



## OpenCV compilation reference articles
- https://solarianprogrammer.com/2018/12/18/cross-compile-opencv-raspberry-pi-raspbian/
- https://www.hackeradam.com/installing-opencv-4-on-windows-10/
- https://www.hackeradam.com/creating-an-opencv-4-project-in-visual-studio-2017/
- https://abhishek4273.com/2014/02/05/install-opencv/
- https://stackoverflow.com/questions/13146650/compiling-opencv-samples-unknown-cmake-command-ocv-check-dependencies
- https://docs.opencv.org/3.3.0/db/df5/tutorial_linux_gcc_cmake.html

## OpenCV links
- https://github.com/opencv/opencv
- ◘https://github.com/shimat/opencvsharp
- https://github.com/opencv/opencv/blob/master/samples/cpp/facedetect.cpp
- 