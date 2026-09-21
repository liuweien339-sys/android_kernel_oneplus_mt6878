SuSFS Kernel for OnePlusTurbo6X
-------
Installation steps:

1. Download AK3 from releases section

2. Flash this AK3 via KernelFlasher or Recovery

3. Waiting for phone`s boot

-------

kernel is 6.1.134 OnePlus kernel base you can boot this at ColorOS16(releases kernel will test on real phone)

-------
Kernel Integrated:

DroidSpaces and SuSFS

-------

Build Steps:

Host Ubuntu24.04

1.Clone Kernel source
```
git clone https://github.com/liuweien339-sys/android_kernel_oneplus_mt6878 kernel
git clone https://github.com/OnePlusOSS/android_kernel_modules_and_devicetree_oneplus_mt6878 modules
```

Remember copy modules repo files
```
cp -r ~/modules/vendor ~/
cp -r ~/modules/kernel ~/
```

2. Install Build deps

```
sudo apt install libssl-dev libelf-dev libdw-dev build-essential dwarves gcc-aarch64-linux-gnu linux-tools-common 
```

cd kernel source

```
cd kernel
```

3.Install Toolchain
```
curl -LO https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/+archive/refs/heads/android14-qpr3-release/clang-r487747c.tar.gz

              mkdir -p toolchains/clang-r487747c
          
              tar -xf "clang-r487747c.tar.gz" -C "toolchains/clang-r487747c"
```

4.Integrate ReSukiSU SuSFS DS patch
```
curl -LSs "https://raw.githubusercontent.com/ReSukiSU/ReSukiSU/main/kernel/setup.sh" | bash
```

```
git clone https://gitlab.com/simonpunk/susfs4ksu.git -b gki-android14-6.1 --depth=1 susfs
              cd susfs
              cp kernel_patches/50_add_susfs_in_gki-android14-6.1.patch ~/kernel
              cp kernel_patches/fs/* ~/kernel/fs
              cp kernel_patches/include/linux/* ~/kernel/include/linux/
              cd ~/kernel
              patch -p1 < 50_add_susfs_in_gki-android14-6.1.patch
              
```

```
curl -Lo ds.patch https://raw.githubusercontent.com/ravindu644/Droidspaces-OSS/refs/heads/main/Documentation/resources/kernel-patches/GKI/below-kernel-6.12/001.GKI-below-6.12-fix_sysvipc_kabi_6_7_8.patch
patch -p1 < ds.patch 
```

5.Make config and export path
```
export PATH="${GITHUB_WORKSPACE}/toolchains/clang-r487747c/bin:$PATH"
          export ARCH=arm64
          export SUBARCH=arm64
          export LLVM=1
          export LLVM_IAS=1
          export CROSS_COMPILE=aarch64-linux-gnu-
```

```
make device_build_
defconfig
```

6. Compile Kernel
```
make -j$(nproc) Image 2>&1 | tee ${GITHUB_WORKSPACE}/build.log
```

Image will execute in arch/arm64/boot/Image

-------
Thanks:

ReSukiSU - @ReSukiSU

susfs4ksu - @simonpunk

DroidSpaces - @ravindu644

-------
