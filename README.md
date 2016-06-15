
#### build guide
Build on ubuntu-15.04-x86_64. 

##### tools
```sh
apt-get update
```
```sh
apt-get install build-essential debian-archive-keyring debootstrap qemu-user qemu-user-static gperf bison flex texinfo help2man gawk libtool libtool-bin automake libncurses5-dev vim python python2.7-dev bc curl cmake git 
```

##### make-3.8
make sure make version is lower 4.0, otherwise the glib-2.14.1 will build failed. So in this example, I have rebuild make from source to target 3.8. 

```sh
wget http://ftp.gnu.org/gnu/make/make-3.80.tar.bz2
tar -jxvf make-3.80.tar.bz2
cd make-3.80
./configure --prefix=/usr && make -j4 && make install
cd
```

##### crosstool-ng-1.22.0
Before build the crosstool-ng, we need switch to normal user due to crosstool not allow for root. In this example, create a user as `dogi`.
```sh
useradd -m -d /home/dogi dogi
```
Switch to `dogi` and build crosstool-ng.
```sh
su - dogi
wget http://crosstool-ng.org/download/crosstool-ng/crosstool-ng-1.22.0.tar.bz2
tar -jxvf crosstool-ng-1.22.0.tar.bz2
cd crosstool-ng
./configure --prefix=`pwd`/crosstool && make -j4 && make install
cd crosstool
wget https://raw.githubusercontent.com/deepkh/arm-cortex_a7-a9-linux-gnueabihf-linaro-4.8.5/master/arm-cortex_a7-linux-gnueabihf-linaro-4.8.5_glibc-2.15
cp arm-cortex_a7-linux-gnueabihf-linaro-4.8.5_glibc-2.15 .config
./ct-ng build
```

If everythin ok, then the toolchain will store in `/home/dogi/ct-ng/arm-cortex_a7-linux-gnueabihf-4.8.5`
