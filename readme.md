Disclaimer:
I am not Responsible for any bricked devices or broken sd cards!!
I am completely a new guy to android developement at this this point but, i somehow managed to learn and build an lineage 17.1 
build for my Pocophone f1. This was a really new learning curve for me so hope u guys to succeed building it for yourself.
These are the links i followed:
https://wiki.lineageos.org/devices/beryllium/build
https://wiki.lineageos.org/extracting_blobs_from_zips.html#extracting-proprietary-blobs-from-block-based-otas
(Note:since i didnt have my device previously running lineage os i just extracted Blobs,
firmware,and all those vendor files form a lineage os build lineage-17.1-20200422-nightly-beryllium-signed.zip 
Downloaded from their official website:https://download.lineageos.org/beryllium)

Just setup your build enviroinment same as that in the link above

I just changed the codes to acording to my directory to extract blobs:
mkdir ~/android/system_dump/
cd ~/android/system_dump/
unzip path/to/lineage-*.zip system.transfer.list system.new.dat*
unzip path/to/lineage-*.zip vendor.transfer.list vendor.new.dat*
sudo apt-get install brotli
brotli --decompress --output=system.new.dat system.new.dat.br
brotli --decompress --output=vendor.new.dat vendor.new.dat.br
git clone https://github.com/xpirt/sdat2img
python sdat2img/sdat2img.py system.transfer.list system.new.dat system.img
python sdat2img/sdat2img.py vendor.transfer.list vendor.new.dat vendor.img
mkdir system/
sudo mount system.img system/
# (i had a vendor image extracted so i executed the next part also!)
sudo rm system/vendor
sudo mkdir system/vendor
sudo mount vendor.img system/vendor/
cd /android/lineage/device/xiaomi/beryllium/
./extract-files.sh ~/android/system_dump/

(now all these blobs are extracted now we are ready to build)
Building code:
(as same as given in the https://wiki.lineageos.org/devices/beryllium/build)
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
ccache -M 50G # (for single build 50G will be more than enough)
export CCACHE_COMPRESS=1 # (this code is Optional for details read description in link above!)
croot # this changes directory to the build 
brunch beryllium
cd $OUT # your twrp flashable zip will be in this location copy it to your phone and flash it!.Enjoy!!!
