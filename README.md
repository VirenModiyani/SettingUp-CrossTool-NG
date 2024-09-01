# 📦 Setting Up CrossTool-NG
This Repository is about the command require &amp; Guide to setup the ToolChain in LINUX for Embedded Systems:

# 🌿 Chapter 1: Setting Up Your Environment

## 🛠️ Step1: Install The Required Library (you may require few more, If you get error you can ind & install missing one):

```
sudo apt install build-essential autoconf bison flex texinfo \
     help2man gawk libtool libtool-bin libtool-doc libncurses5-dev python3-dev \
     python3-distutils git unzip

```


Get the Repo into your system:
```
git clone https://github.com/crosstool-ng/crosstool-ng.git
cd crosstool-ng
```
Going to the official Repo you can find the latest updated version as "Releases":
```
git checkout crosstool-ng-1.26.0
```

## 🔄 Step 2: Bootstrap and Configure

Run the bootstrap script to set up the build environment:
```
./bootstrap
```
Next, configure the build system:

` The `--enable-local` option installs the program in the current directory, eliminating the need for root permissions. This is in contrast to the default installation path, `/usr/local/bin`, which would require elevated privileges.`
```
./configure --enable-local
```
## 🌐  Step 3: Build and Install

Compile and install Crosstool-NG:
```
make
sudo make install
```

## ✅ Step 4: Verify the Installation

After installation, you can verify that Crosstool-NG is installed correctly by checking its version:
```
./ct-ng version
```
# 🕹️ Chapter 2: Board Selection
## ⚙️ Step 1: Board finding & selecing

After getting the crosstool-ng we need to get the Board.
For that we need to see the list:
```
./ct-ng list-samples
```
We are going to select __arm-unknown-linux-gnueabi.__ in our case. Running distclean first to make sure that there are no artifacts left over from a previous build
```
./ct-ng distclean
```
Selecting board:
```
./ct-ng arm-unknown-linux-gnueabi
```

## 🚀 Step 2: Final Build for the BOARD

⚠️There is only one change is necessary:


In Paths and misc options, disable Render the toolchain read-only

```
/ct-ng menuconfig
```

After disable is done run:
```
./ct-ng build
```

__The toolchain will be installed in ~/x-tools/arm-unknown-linux-gnueabi.😊__

# 🔗 Chapter 3: Explore Toolchain 
`We have Created the toolchain which contains various stuffs as Cross Compiler, Header Files, etc.`

## 🔍 Step 1: To find the files(Compiler & all)
```
cd ~/x-tools/arm-unknown-linux-gnueabi/bin
```

Here you will find our Compiler :         __arm-unknown-linux-gnueabi-gcc__
## 🌐 Step 2: Globally access
To Use our Compiler friend anywhere we make it global as setting path:

```
PATH=~/x-tools/arm-unknown-linux-gnueabi/bin:$PATH
```

_Now it is globally accessible_
## 🧪 Step 3: Testing
To try it out got for new folder any where & make a C file.
```
nano 1.c
```
Paste the code (If you are Lazy):
```
#include <stdio.h>
int main (int argc, char *argv[])
{
printf ("Hello, world!\n");
return 0;
}
```


To Compile it.
```
arm-unknown-linux-gnueabi-gcc 1.c -o 1
```

🎉 You compile code for arm by using x86🤓

Let's try to run it 
```
./1
```

It shows Problem as the code is ment for arm base architecture.

Again to sett the type of file is:
```
file 1
```

This shows the file is formed for which device.

# 🔧 Chapter 4: The ToolChain sysroot (Info)
