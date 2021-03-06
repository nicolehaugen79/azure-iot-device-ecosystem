# Prepare your development environment

This document describes how to prepare your development environment to use the *Microsoft Azure IoT SDK for Python*.

- [Setup your development environment](#devenv)
- [Build the samples on Linux](#linux)
- [Install on Windows using PyPI wheels](#windows-wheels)
- [Build the samples on Windows using nuget packages](#windows)
- [Build the samples on Windows using cmake and boost libraries](#windows-cmake)
- [Sample applications](#samplecode)

<a name="devenv"/>
## Setup your development environment

Complete the following steps to set up your development environment:

- Follow the instructions to setup [Microsoft Azure IoT device SDK for C][setup-devbox].
- Ensure that the desired Python version is installed (2.7.x, 3.4.x or 3.5.x). Run `python --version` or `python3 --version` at the command line to check the version. 
    - On Linux, Python 2.7 is typically already installed and active. 
    - On Windows, install the latest x86 or x64 Python 2.7 or 3.x client from ([python.org](https://www.python.org/downloads/)). The build needs a valid Python.exe in the path. Based on the active Python version (e.g. Python 2.7.11 x86 32bit) the build script choses the compiler settings for the Python extension module build accordingly and copies the extension to the test and sample folder.
- When you have installed Python, make sure you cloned the latest version of this repository ([azure-iot-sdk-python](https://github.com/Azure/azure-iot-sdk-python)) to your development machine or device. You should always use the **master** branch for the latest version of the libraries and samples.

<a name="linux"/>
## Build the Python iothub_client module on Linux

The Python module builds on the Azure IoT device SDK for C. Please make sure you can build the C samples as described in [setup-devbox] before you continue in this section.

The Python iothub_client supports python versions 2.7.x, 3.4.x or 3.5.x. Know the appropriate version you would like to build the library with for the following instructions.

1. Ensure that the desired Python version (2.7.x, 3.4 or 3.5.x) is installed and active. Run `python --version` or `python3 --version` at the command line to check the version.
2. Open a shell and navigate to the folder **python/build_all/linux** in your local copy of the repository.
3. Run the `./setup.sh` script to install the prerequisite packages and the dependent libraries.
    * Setup will default to python 2.7
    * To setup dependencies for python 3.4 or 3.5, run `./setup.sh --python-version 3.4` or `./setup.sh --python-version 3.5` respectively
4. Run the `./build.sh` script.
    * Build will default to python 2.7
    * To build with python 3.4 or 3.5, run `./build.sh --build-python 3.4` or `./build.sh --build-python 3.5` respectively 
5. After a successful build, the `iothub_client.so` Python extension module is copied to the **python/device/samples** folder. Please follow instructions in [Sample applications](#samplecode) to run the Python samples.

###Known build issues: 

1.) On building the Python client library (`iothub_client.so`) on Linux devices that have less than **1GB** RAM, you may see build getting **stuck** at **98%** while building `iothub_client_python.cpp` as shown below

``[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o``

If you run into this issue, check the **memory consumption** of the device using `free -m command` in another terminal window during that time. If you are running out of memory while compiling iothub_client_python.cpp file, you may have to temporarily increase the **swap space** to get more available memory to successfully build the Python client side device SDK library.

2.) CentOS7: Only Python 2.7 is supported due to a missing boost-python3 library package

<a name="windows-wheels"/>
## Install the Python iothub_client module on Windows from [PyPI] 

The following instructions outline how you can install the **iothub\_client** module in Windows from [PyPi] using the **iothub\_client** package:

1. Open a command-prompt window.
2. Install the **iothub\_client** package with the following command: `pip install iothub-client`
3. Now Python is ready to run the sample applications. 
Please follow instructions in [Sample applications](#samplecode) to run the Python samples.

> Note: If it is not already installed, you need to install the [Visual C++ Redistributable for Visual Studio 2015][lnk-c-redist].

> Note: If Pip cannot install the package for the specific version of Python installed on your machine, use one of the following options to build the **iothub_client** module.

<a name="windows"/>
## Build the Python iothub_client module on Windows using Nuget packages (recommended)

The following instructions outline how you can build the libraries in Windows:

1. Open a Visual Studio 2015 x86 Native Tools command prompt and navigate to the folder **python/build_all/windows** in your local copy of the repository.
2. Run the script `build.cmd` in the **python\\build_all\\windows** directory.
3. As a result, the `iothub_client.pyd` Python extension module is copied to the **python/device/samples** folder. Follow the instructions in [Sample applications](#samplecode) to run the Python IoT Hub samples.
4. In order to run the samples with a different Python version (e.g. 32bit vs. 64bit or 2.7 vs. 3.4) please rebuild the `iothub_client.pyd` extension.

<a name="windows-cmake"/>
## Build the Python iothub_client module on Windows using cmake and boost libraries 

###The following instructions outline how you prepare the boost::python libraries in Windows. 
This step is only required once and only necessary for the cmake build:

1. Open a Visual Studio 2015 x86 or x64 Native Tools command prompt, depending on your installed Python version.
2. Download the zip version of the boost 1.60 library for windows from [boost-zip]. 
3. Unpack zip to a local folder, e.g. **C:\boost_1_60_0**.
4. Navigate to the root folder of the boost library, e.g. **C:\boost_1_60_0**.
5. Run the script `bootstrap.bat` in the boost folder.
6. At this point the desired Python version (e.g. V3.4 64bit) must be installed and active in the `PATH` environment variable.
7. Run the build command `b2` in the same folder. For x64 builds use the build command `b2 address-model=64`.
8. Set the environment variable **BOOST_ROOT** to the boost folder using the command `SET BOOST_ROOT=C:\boost_1_60_0`.

Now the boost::python library is ready for use. 

Important note: The boost libraries can only be used to build for a single Python version (e.g. V3.4 64bit). In order to build with cmake for another Python version, a clean bootstrap and build of the boost libraries for that version is necessary, or build errors will occur. It is possible to have multiple boost libraries for different Python versions side by side in different roots. Then the **BOOST_ROOT** folder must point to the appropriate boost library at cmake build time.

###The following instructions outline how you can build the Python extension module with cmake in Windows:

1. Open a Visual Studio 2015 x86 Native Tools command prompt and navigate to the folder **python/build_all/windows** in your local copy of the repository.
2. Verify that `BOOST_ROOT` points to the local boost installation for the right Python version.
3. Run the script `build_client.cmd` in the **python\\build_all\\windows** directory.
4. After a successful build, the `iothub_client.pyd` Python extension module is copied to the **python/device/samples** folder. Please follow instructions in [Sample applications](#samplecode) to run the Python samples.

To use the iothub_client extension for native code debugging with [Python Tools for Visual Studio] run the script: `build_client.cmd --config Debug` to get the full debug symbol support.

<a name="samplecode"/>
## Sample applications

This repository contains various Python sample applications that illustrate how to use the Microsoft Azure IoT SDK for Python. To learn how to run a sample application that sends messages to an IoT hub, see [Getting started - running a Python sample application][getstarted].

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]   

[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[python-2.7 or python-3.5]: https://www.python.org/downloads/
[PyPI]: https://pypi.python.org/pypi/iothub-client/
[Python Tools for Visual Studio]: https://www.visualstudio.com/en-us/features/python-vs.aspx
[setup-devbox]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[getstarted]: python-run-sample.md
[boost-zip]: http://www.boost.org/users/history/version_1_60_0.html
[lnk-c-redist]: https://www.microsoft.com/download/details.aspx?id=48145
