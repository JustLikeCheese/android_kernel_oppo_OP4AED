# Setup env
```sh
sudo apt-get update
sudo apt-get install -y curl git ftp lftp wget libarchive-tools ccache python2.7 python3
sudo apt-get install -y pngcrush schedtool dpkg-dev liblz4-tool make optipng maven device-tree-compiler 
sudo apt-get install -y libc6-dev-i386 lib32ncurses5-dev libx11-dev lib32z-dev libgl1-mesa-dev xsltproc
sudo apt-get install -y libxml2-utils libbz2-dev libbz2-1.0 libghc-bzlib-dev squashfs-tools lzop flex tree
sudo apt-get install -y build-essential bc gcc-aarch64-linux-gnu gcc-arm-linux-gnueabi libssl-dev libfl-dev
sudo apt-get install -y pwgen libswitch-perl policycoreutils minicom libxml-sax-base-perl libxml-simple-perl 
sudo apt-get install -y zip unzip tar gzip bzip2 rar unrar llvm g++-multilib bison gperf zlib1g-dev automake
sudo apt install -y bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5-dev libncurses5 libsdl1.2-dev libssl-dev libwxgtk3.0-gtk3-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev make unzip python-is-python3
```

# Setup Swap
```sh
sudo fallocate -l 10G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

# Git Clone Android Kernel Sources
```sh
cd
git clone --depth=1 --branch master https://github.com/JustLikeCheese/android_kernel_oppo_OP4AED.git OPPOA8
```

# Get Toolchains
```sh
mkdir -p toolchains && cd toolchains
git clone --depth=1 --branch master https://github.com/Chengrose/clang-r353983d.git clang
git clone --depth=1 --branch master https://github.com/Chengrose/arm64_for_OP4AED.git aarch64
git clone --depth=1 --branch master https://github.com/Chengrose/arm_for_OP4AED.git arm
```

# CCache
```sh
cd
mkdir -p ccache
export CCACHE_DIR="$PWD/ccache"
ccache -M 10G
```

# Precompile
```sh
export ARCH=arm64
export CLANG_TRIPLE=aarch64-linux-gnu-
export CROSS_COMPILE=aarch64-linux-androidkernel-
export CROSS_COMPILE_ARM32=arm-linux-androidkernel-
export PATH="$PWD/toolchains/clang/bin:$PWD/toolchains/aarch64/bin:$PWD/toolchains/arm/bin:$PATH"
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
MAKE_ARGS="
    -j$(nproc) \
    O=out \
    ARCH=$ARCH \
    CC=clang \
    CLANG_TRIPLE=$CLANG_TRIPLE \
    CROSS_COMPILE=$CROSS_COMPILE \
    CROSS_COMPILE_ARM32=$CROSS_COMPILE_ARM32 \
    AR=llvm-ar \
    NM=llvm-nm \
    OBJCOPY=llvm-objcopy \
    STRIP=llvm-strip
"
```

# Build Kernel
```sh
cd ~/OPPOA8
make clean && rm -rf out
make $MAKE_ARGS oppo6765_19181_defconfig
make $MAKE_ARGS
```

# AnyKernel
```sh
cd
git clone https://github.com/osm0sis/AnyKernel3.git
```
```sh
cp OPPOA8/out/arch/arm64/boot/Image.gz-dtb AnyKernel3/
```
```sh
cd AnyKernel3
zip -r9 OPPOA8-Kernel-$(date +%Y%m%d).zip *
```
