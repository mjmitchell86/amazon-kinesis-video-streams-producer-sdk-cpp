Amazon Kinesis Video Streams Producer SDK CPP

## License

This library is licensed under the Amazon Software License.

## Introduction
Amazon Kinesis Video Streams makes it easy to securely stream video from connected devices to AWS for analytics, machine learning (ML), and other processing.

Amazon Kinesis Video Streams Producer SDK for C/C++ makes it easy to build an on-device application that securely connects to a video stream, and reliably publishes video and other media data to Kinesis Video Streams. It takes care of all the underlying tasks required to package the frames and fragments generated by the device's media pipeline. The SDK also handles stream creation, token rotation for secure and uninterrupted streaming, processing acknowledgements returned by Kinesis Video Streams, and other tasks.

Amazon Kinesis Video Streams Producer SDK for C/C++ contains the following sub-directories/projects:
* kinesis-video-pic - The Platform Independent Codebase which is the basic building block for the C++/Java producer SDK. The project includes multiple sub-projects for each sub-component with unit tests.
* kinesis-video-producer - The C++ Producer SDK with unit test.
* kinesis-video-producer-jni - The C++ wrapper for JNI to expose the functionality to Java/Android.
* kinesis-video-gst-demo - C++ GStreamer sample application.
* kinesis-video-native-build - Native build directory with a build script for Mac OS/Linux/Raspberry PI. This is the directory that will contain the artifacts (executable binaries) and JNI libraries after the build.

----

## Building from Source
There are few build-time tools/dependencies which need to be installed in order to build the core SDK libraries and the samples.

### Build Dependencies
Please install the following additional build tools before proceeding with `./install-script`(the example below is for **Mac OS X**). One option is to use `brew install` the following dependencies.

* autoconf 2.69 (License GPLv3+/Autoconf: GNU GPL version 3 or later) http://www.gnu.org/software/autoconf/autoconf.html
* cmake 3.7/3.8 https://cmake.org/
* flex 2.5.35 Apple(flex-31)
* bison 2.4 (GNU License)
* automake 1.15.1 (GNU License)
* libtool (Apple Inc. version cctools-898)
* xCode (Mac OS) / clang / gcc (xcode-select version 2347)
* Java jdk (for Java JNI compilation)

After you've downloaded the code from GitHub, you can build it on Mac OS or Ubuntu using `./install-script`  (which is inside the `kinesis-video-native-build` directory).

**Important** Change *current working directory* to the `kinesis-video-native-build` directory first. Then run the `./install-script` from that directory.
  
This will produce the core library, the JNI library, unit tests executable and the sample GStreamer application. The script will download and build the dependent open source components in the 'downloads' directory (within `kinesis-video-native-build` directory)and link against it.
 
#### Build the binaries using system versions
The bulk of the install script is building the open source dependencies. The project is based on **CMake**. So the open source components building can be skipped if the system versions can be used for linking. 

Running 

```
$ cmake . 
$ make
```
from the `kinesis-video-native-build` directory will build and link the SDK. 
The `./min-install-script` inside the `kinesis-video-native-build` captures these steps for installing the Kinesis Video Streams Producer SDK with the system versions for linking.

#### Build the binaries using the dependent libraries from source


The `install-script` will take some time to bring down and compile the open source components. If anything fails or the script is interrupted, re-running it will pick up from the place where it last left off. The sub-sequent run will be building just the modified SDK components or applications which is much faster.

#### Build the native library (KinesisVideoProducerJNI) to run Java Demo App
The `./java-install-script` inside `kinesis-video-native-build` will build the KinesisVideoProducerJNI native library to be used by [Java Producer SDK](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-java/blob/master/README.md).


----

### Open Source Dependencies
The projects depend on the following open source components. Running `install-script` will download and build the necessary components automatically.

#### Core
* openssl (crypto and ssl) - [License](https://github.com/openssl/openssl/blob/master/LICENSE)
* curl lib - [Copyright](https://curl.haxx.se/docs/copyright.html)
* log4cplus - [License]( https://github.com/log4cplus/log4cplus/blob/master/LICENSE)
* jsoncpp - [License](https://github.com/open-source-parsers/jsoncpp/blob/master/LICENSE)

#### Unit Tests
* [googletest](https://github.com/google/googletest)

#### GStreamer Demo App
* gstreamer - [License]( https://gstreamer.freedesktop.org/documentation/licensing.html)
* gst-plugins-base
* gst-plugins-good
* gst-plugins-bad
* gst-plugins-ugly
* [x264]( https://www.videolan.org/developers/x264.html)

#### Certificate store integration
Kinesis Video Streams Produicer SDK for C++ needs to establish trust with the backend service through TLS. This is done through validating the CAs in the public certificate store. On Linux-based models, this store is located in /etc/ssl/ directory by default.

Please download the PEM file from
https://www.amazontrust.com/repository/SFSRootCAG2.pem

to `/etc/ssl/cert.pem`. Append to the end of the file if it exists.

Many platforms come with a cert file with a lot of the well-known public certs in them.

----


## Install Steps for Ubuntu 17.x using apt-get
The following are the steps to install the build-time prerequisites for Ubuntu 17.x

Install **git**: 

``` 
$ sudo apt-get install git
$ git --version
git version 2.14.1
```
Install **cmake**: 
```
$ sudo apt-get install cmake
$ cmake --version
cmake version 3.9.1
```
CMake suite maintained and supported by Kitware (kitware.com/cmake).

Install **libtool**:  (some images come preinstalled) amd **libtool-bin**
```
$ sudo apt-get install libtool
$ sudo apt-get install libtool-bin
$ libtool --version
libtool (GNU libtool) 2.4.6
Written by Gordon Matzigkeit, 1996

Copyright (C) 2014 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
Install **automake**: 
```
$ sudo apt-get install automake
$ automake --version
automake (GNU automake) 1.15
Copyright (C) 2014 Free Software Foundation, Inc.
License GPLv2+: GNU GPL version 2 or later <http://gnu.org/licenses/gpl-2.0.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Tom Tromey <tromey@redhat.com>
       and Alexandre Duret-Lutz <adl@gnu.org>.
```
Install **g++**: 
```
$ sudo apt-get install g++
$ g++ --version
g++ (Ubuntu 7.2.0-8ubuntu3) 7.2.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
Install **curl**: 
```
$ sudo apt-get install curl
$ curl --version
curl 7.55.1 (x86_64-pc-linux-gnu) libcurl/7.55.1 OpenSSL/1.0.2g zlib/1.2.11 libidn2/2.0.2 libpsl/0.18.0 (+libidn2/2.0.2) librtmp/2.3
Release-Date: 2017-08-14
Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtmp rtsp smb smbs smtp smtps telnet tftp
Features: AsynchDNS IDN IPv6 Largefile GSS-API Kerberos SPNEGO NTLM NTLM_WB SSL libz TLS-SRP UnixSockets HTTPS-proxy PSL
```
Install **pkg-config**: 
```
$ sudo apt-get install pkg-config
$ pkg-config --version
0.29.1
```
Install **flex**: 
```
$ sudo apt-get install flex
$ flex --version
flex 2.6.1
```
Install **bison**:
```
$ sudo apt-get install bison
$ bison -V
bison (GNU Bison) 3.0.4
Written by Robert Corbett and Richard Stallman.

Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
Install **Open JDK**: 
```
$ sudo apt-get install openjdk-8-jdk
$ java -showversion
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-8u151-b12-0ubuntu0.17.10.2-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```
Set **JAVA_HOME** environment variable: 
```
$ export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/
```
Run the build script: (within `kinesis-video-native-build` folder) 
```
./install-script
```
----
## Examples

### Running the sample demo applications

#### Setting credentials in environment variables
Define AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY environment variables with the AWS access key id and secret key:

```
$ export AWS_ACCESS_KEY_ID={AccessKeyId}
$ export AWS_SECRET_ACCESS_KEY={SecretAccessKey}
```
optionally, set `AWS_SESSION_TOKEN` if integrating with temporary token and `AWS_DEFAULT_REGION` for the region other than `us-west-2`

#### GStreamer webcam demo application

The GStreamer demo app will be built in `kinesis_video_gstreamer_sample_app` in the `kinesis-video-native-build` directory. Launch it with a stream name and it will start streaming from the camera. The user can also supply a streaming resolution (width and height) through command line arguments.

```
Usage: AWS_ACCESS_KEY_ID=<SAMPLEKEY> AWS_SECRET_ACCESS_KEY=<SAMPLESECRET> ./kinesis_video_gstreamer_sample_app <my-stream-name> -w <width> -h <height> -f <framerate> -b <bitrateInKBPS>
```
* **A.** If resolution is provided then the sample will try to check if the camera supports that resolution. If it does detect that the camera can supprt the resolution supplied in command line, then streaming starts; else, it will fail with an error msg `Resolution not supported`
  
* **B.** If no resolution is specified, the demo will try to use these three resolutions **1920x1080, 1280x720 and 640x480** in that order (highest resolution first) and will **start streaming** once the camera supported resolution is detected.

#### GStreamer RTSP demo application

The GStreamer RTSP demo app will be built in `kinesis_video_gstreamer_sample_rtsp_app` in the `kinesis-video-native-build` directory. Launch it with a stream name and `rtsp_url`  and it will start streaming.

```
AWS_ACCESS_KEY_ID=<ACCESS_KEY> AWS_SECRET_ACCESS_KEY=<SECRET_KEY> ./kinesis_video_gstreamer_sample_rtsp_app <my_rtsp_url> my-rtsp-stream

```

#### Running C++ Unit tests

The executable for **unit tests** will be built in `./start` inside the `kinesis-video-native-build` directory. Launch it and it will run the unit test and kick off dummy frame streaming.



#### Enabling verbose logs
Define `HEAP_DEBUG` and `LOG_STREAMING` C-defines by uncommenting the appropriate lines in CMakeList.txt in the kinesis-video-native-build directory.

#### Additional Examples

For additional examples on using Kinesis Video Streams Java SDK and  Kinesis Video Streams Parsing Library refer: 

##### [Kinesis Video Streams Producer Java SDK](https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-java/blob/master/README.md)
##### [Kinesis Video Streams Parser Library](https://github.com/aws/amazon-kinesis-video-streams-parser-library/blob/master/README.md)
##### [Kinesis Video Streams Android SDK](https://github.com/awslabs/aws-sdk-android-samples/tree/master/AmazonKinesisVideoDemoApp)
----


----

### Troubleshooting

##### Ubuntu builds link issues
Ubuntu bulds link against the system versions of the open source component libraries or missing .so files (./start in the kinesis-video-native-build directory shows linkage against system versions of the open source libraries).We are working on providing fix but the immediate steps to remedy is to run 

```
  rm -rf ./kinesis-video-native-build/CMakeCache.txt ./kinesis-video-native-build/CMakeFiles
``` 
  and run 
```
 ./install-script
``` 
   to rebuild and re-link the project only.

##### Raspberry PI failure to load the camera device.

To check this is the case run `ls /dev/video*` - it should be file not found. The remedy is to run the following:
```
$ls /dev/video*
{not found}
```
```
$vcgencmd get_camera 
```
Example output:
```
supported=1 detected=1
```
if the driver does not detect the camera then

* Check for the camera setup and whether it's connected properly
* Run firmware update `$ sudo rpi-update` and restart
```
$sudo modprobe bcm2835-v4l2
```
```
$ls /dev/video* 
{lists the device}
```

##### Raspberry PI timestamp/range assertion at runtime.

Update the Raspberry PI firmware.
```
$ sudo rpi-update
$ sudo reboot
```

* Raspberry PI GStreamer assertion on gst_value_set_fraction_range_full: assertion 'gst_util_fraction_compare (numerator_start, denominator_start, numerator_end, denominator_end) < 0' failed. The uv4l service running in the background. Kill the service and restart the sample app.


##### Raspberry PI seg fauls after some time running on `libx264.so`. 
Rebuilding the `libx264.so` library and **re-linking the demo application** fixes the issue.

##### Curl SSL issue - "unable to get local issuer certificate"
If curl throws *"Peer certificate cannot be authenticated with given CA certificates: SSL certificate problem: unable to get local issuer certificate"* error while sending data to KVS, then the local `curl`
was not built properly with `--with-ca-bundle` path. So please remove the curl binaries and libraries and rebuilt it again by following below steps.
```
rm <producer_sdk_path/kinesis-video-native-build/downloads/local/lib/libcurl*
rm <producer_sdk_path/kinesis-video-native-build/downloads/local/bin/curl*
cd <producer_sdk_path/kinesis-video-native-build/downloads/curl-7.57.0
export DOWNLOADS=<producer_sdk_path>/kinesis-video-native-build/downloads
make clean
./configure --prefix=$DOWNLOADS/local/ --enable-dynamic --disable-rtsp --disable-ldap --without-zlib --with-ssl=$DOWNLOADS/local/ --with-ca-bundle=/etc/ssl/cert.pem
make
make install
```

----


## Release Notes
#### Release 1.2.3 (1st March 2018)
* Updated install-script to fix the local certificate trust issue for curl.
* Added steps in README troubleshooting section for curl trust issues.
#### Release 1.2.2 (March 2018)
* Remove open-source dependencies from KinesisVideoProducerJNI native library. java-install-script can be used to build KinesisVideoProducerJNI native library fast.
* README note improved.
#### Release 1.2.1 (February 2018)
* Bug fix for producer timestamp *video playback* in the console should be fixed if proper timestamp is provided to SDK. Current setting in sample app uses Gstreamer frame timecode and relative timestamp (used by SDK).
* `install-script` is updated to automatically detect OS version and avoid dependency issue on Mac High Sierra and Ubuntu 17.10.
* Known issue: Producer timestamp mode *video playback in console* will not work if GStreamer demoapp is configured to use frame timecode and absolute timestamp (used by SDK).
#### Release 1.2.0 (February 2018)
* Bug fixes and performance enhancement
* Streaming error recovery improvements
* Minor API changes:
  * create stream APIs return shared pointers instead of unique pointers
  * Addition of StreamClosed callback to notify the caller application when the stream is finished draining the existing buffered frames before closing in the graceful termination case.
#### Release 1.1.3 (February 2018)
* Added **RTSP Demo Sample**
* Run the demo using:
```
AWS_ACCESS_KEY_ID=<MYACCESSKEYID> AWS_SECRET_ACCESS_KEY=<MYSECRETKEY> ./kinesis_video_gstreamer_sample_rtsp_app <rtspurl> <stream-name>
```
#### Release 1.1.2 (January 2018)
* Allowed devices to output h.264 streams directly
* The user can also supply a streaming resolution through command line arguments.
  * If resolution is provided then the sample will try to check if the camera supports that resolution. If it does then streaming starts; else, it will fail with an error msg "Resolution not supported"
  * If no resolution is specified, the demo will try to use resolutions 1920x1080, 1280x720 and 640x480 in that order (highest resolution first)and will start streaming once the camera supported resolution is detected.
* Known issues:
  * When streaming on raspberry pi. Some green artifacts might be observed on the preview screen. Reducing the resolution can fix the issue.
#### Release 1.1.1 (December 2017)
* Fix USB webcam support
* Known issues:
    * If USB webcam doesn't support 720p, then gstreamer negotiation will fail. Trying lower resolution as mentioned in Troubleshooting may fix this issue.
#### Release 1.1.0 (December 2017)
* Addition of a received application ACK notification callback
* Lifecycle management improvements
* Exposed failure on progressive back-off/retry logic
* Hardening/fixing various edge-cases
* Fixing Raspberry PI frame dropping issue
#### Release 1.0.0 (November 2017)
* First release of the Amazon Kinesis Video Producer SDK for Cpp.
* Known issues:
    * Missing build scripts for Windows-based systems.
    * Missing cross-compile option.
    * Sample application/unit tests can't handle buffer pressures properly - simple print in debug log.

## Documentation

  [Kinesis Video Producer SDK CPP](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/producer-sdk-cpp.html)

