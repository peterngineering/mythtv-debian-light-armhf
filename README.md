# mythtv-debian-light-armhf

* * *

This ongoing project is a fork of upstream 'mythtv & packaging' projects
from specific commit(s) with some minor changes to optimize and build
debian packages for a specific cpu.

**Note: I am not associated with either upstream project or official
developers of such in any way.**

## Why bother when official developers elsewhere offer 64bit versions?

1. _The primary goal of this so far is to make debian packages easily 
available for older 32bit hardware such as the rpi2/rpizero. Loads of
rpizero/rpi2 devices are still around. Debian still supports armhf
at least until 2030 with Trixie(LTS)._

3. _Make it easy to rework this for any cpu and rebuild._

4. _It is fun to build and see it working on low end arms that can
be as low as 1W(not including tuner). AFAIK, the arm6/arm11 rpizero
the original but still in production, even though over 10 years old is 
one of the least expensive devices that can run a full OS with mythtv.
The rpi2(cortex-a7) offers quite a bit more power for about 2-2.5
watts as well. All of the newer 64bit devices from my experience 
will use 2x more power than these older devices respectively._


## How to setup your development environment.
_Assuming you wanted to build for armhf types:_
I recommend you use a rpi4/5 or similar hw, booted with 32bit userland
and kernel. 


### Create a ~/.buildrc
<code>
BUILD_TYPE=Release
BUILD_PRESET=qt5
BUILDDEST=~/proj/build
BUILD_METHOD=make
</code>

### Create a 'WORK' Directory(or similar) for the cloned repos
<code>mkdir ~/WORK</code>


### Install dev dependencies and a few other useful tools:
_(The following only tested on Debian Bookworm so far)_
<code>
apt install git-core btop nmap screen vim sudo dh-exec ccache cmake \
ninja-build pkgconf libdrm-dev libfreetype-dev libfontconfig-dev \
libxml2-dev libmp3lame-dev libvorbis-dev libflac-dev libzip-dev libtag1-dev \
libbluray-dev libsamplerate-dev libsoundtouch-dev libqt5sql5-mysql libqt5opengl5-dev \
libxrandr-dev libasound2-dev libhdhomerun-dev libavahi-compat-libdnssd-dev \
libssl-dev libcdio-dev libcdio-paranoia-dev libnet-upnp-perl libio-socket-inet6-perl \
liblwp-useragent-determined-perl libdbd-mysql-perl libhttp-parser-perl libxml-simple-perl \
libmodule-build-perl libhttp-message-perl libdbd-mysql-perl libdbi-perl libimage-size-perl \
libdatetime-format-iso8601-perl libsoap-lite-perl libjson-perl libdate-manip-perl libxml-xpath-perl \
python3-lxml python3-mysqldb python3-setuptools python3-pycurl -y</code>

### Enter your 'WORK' directory 
<code>cd ~/WORK</code>

### Clone this repo's specific the branch you want to build
<code>git clone --single-branch -b rpi2/fixes/35 https://github.com/peterngineering/mythtv-debian-light-armhf</code>

### Clone the mythtv upstream specific branch you want to build
## Make sure it matches the version above, eg. <rpi2/fixes/35> and <fixes/35>
<code>git clone --single-branch -b fixes/35 https://github.com/MythTV/mythtv</code>

### Optionally, copy over a custom optimized configure file with new ffmpeg options specific to your cpu.
### TESTING OF THIS SECTION INCOMPLETE may produced unexpected results and/or mark your packagename as "-dirty"
### If in doubt skip this section.
<code>cp -av ~/WORK/mythtv-debian-light-armhf/OPTIONAL_MYTHTV35_CONFIGURE_DEB-LIGHT-RPI2.configure ~/WORK/mythtv/mythtv/configure</code>

This step adds a new configure file with section modification for the rpi2 example project here. 
_YMMV. But, This is intended to further reduce file sizes of executables
and libraries and to hopefully increase performance on cpu constrained
devices._
 
<code>
--enable-neon \
--arch=armv7-a \
--cpu=cortex-a7 \
--extra-cflags=-mfpu=neon \
--extra-cxxflags=-mfpu=neon \
--enable-small \
--disable-debug \
--disable-hardcoded-tables \
--disable-runtime-cpudetect \
</code>


### Build the main mythtv package from the mythtv branch you cloned, and call the build_package.sh script directly from it.
<code>cd ~/WORK/mythtv/mythtv</code>

<code>../../mythtv-debian-light-armhf/deb-light-rpi2/package.sh</code>

_If all went well you should have a new deb produced in 'WORK' for the main mythtv package._

### Build the plugins:
<code>cd ../mythplugins</code>

<code>../../mythtv-debian-light-armhf/deb-light-rpi2/package.sh</code>

_If all went well you should have a new deb produced in 'WORK' for the mythplugins package._

_Both the rpi2 and  rpizero can do a mythbackend , but
com flagging will be slow. You might even have to disable it for
the rpizero so that it doesnt interfere with serving up UPnP streams._




**Tip on using the rpizero as a frontend.**
Due to the single cpu and no NEON on the arm6/arm11 getting this working
as a frontend is going to be very specific.

_It will not work to any level of satisfication unless you configure
the playback profile for 'v4l2 codec'._
<code>
Current Video Playback Profile  'V4L2 Codecs with V4L2 acceleration and 
OpenGL Hardware, with decoder on "V4L2 accel", 1 cpu,
Video Renderer= "OpenGL Hardware" Dint HQ, with no 2x Deint
</code>

_Now if you have the profile configured correctly and your using mp4 
sources such as those from a HDHR-extend, it will work for you
with 480 & 720 source resolutions at 30fps. 
Even so, it will stutter when your in the menu._

_I have not tested this scenario yet with using ota mpg2 sources, but I
suspect 480 SD may work_



**Using the rpi2 as a frontend** 
_You can use ffmpeg profiles as well since the the extra cpu cores and 
memory can handle it easily. You may decide to use v4l2 even with this 
anyway so that you could reserve cpu power for the menuing system 
and other tasks._









_I expect to add systemd service files for frontend and backend in the
future._










