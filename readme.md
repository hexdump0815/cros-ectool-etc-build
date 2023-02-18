# cros-ectool-etc-build

notes about building the chomeos ectool etc. on a mainline linux system running on a chromebook

IMPORTANT: be very careful when playing around with this - it might break things in bad ways
           everything you do is on your own risk!

# all this is based on some initial experments mps did on his elm chromebook on alpine

apt-get -y install libftdi1-dev libusb-1.0-0-dev
mkdir -p /compile/source
cd /compile/source
git clone https://chromium.googlesource.com/chromiumos/platform/ec
cd ec
# use "git branch -r | grep release-R" to see which useable branches are available
git checkout release-R105-14989.B-main
# some patches are required to make it build on a debian system for instance
patch -p1 < compile-fixes.patch
# now adjust the BOARD variable in the Makefile to the board name the ectool should
# be built for - default is krane, which is the board name of the lenovo duet chromebook 10.1
make HOST_CROSS_COMPILE= utils-host
# results will be in ./build/krane/util i.e. build/<boardname>/util

potentially interesting commands:
ectool batterycutoff at-shutdown - will disconnect the battery after shutdown, good for long
                                   term storage to avoid battery drain, power supply has to
                                   be connected to bring the chromebook back to live
ectool chargecontrol - can control the battery charging in some way, might be the base for
                       some daemon or shell script to keep the battery charge always in a
                       certain range
