# mythtv-debian-light-armhf
# RPI4
* * *

This ongoing project is a fork of upstream 'mythtv & packaging' projects
from specific commit(s) with some minor changes to optimize and build
debian packages for a specific cpu.

**Note: I am not associated with either upstream project or official
developers of such in any way.**


## How to setup your development environment.
_Assuming you wanted to build for armhf types:_
I recommend you use a rpi4 or similar hw, booted with 32bit userland
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
<code>git clone --single-branch -b rpi4/fixes/35 https://github.com/peterngineering/mythtv-debian-light-armhf</code>

### Clone the mythtv upstream specific branch you want to build
## Make sure it matches the version above, eg. <rpi4/fixes/35> and <fixes/35>
<code>git clone --single-branch -b fixes/35 https://github.com/MythTV/mythtv</code>

### Build the main mythtv package from the mythtv branch you cloned, and call the build_package.sh script directly from it.
<code>cd ~/WORK/mythtv/mythtv</code>

<code>../../mythtv-debian-light-armhf/deb-light-rpi4/package.sh</code>

_If all went well you should have a new deb produced in 'WORK' for the main mythtv package._

### Build the plugins:
<code>cd ../mythplugins</code>

<code>../../mythtv-debian-light-armhf/deb-light-rpi4/package.sh</code>

_If all went well you should have a new deb produced in 'WORK' for the mythplugins package._



_I expect to add systemd service files for frontend and backend in the
future._

