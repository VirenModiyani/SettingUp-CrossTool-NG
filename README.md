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
To Use our Compiler friend anywhere we make it global as setting path :

```
nano ~/.bashrc
```
Paste at the end of script (Thisscript always gets executed when the terminal starts)
```
export PATH=~/x-tools/arm-unknown-linux-gnueabi/bin:$PATH
```
Save and exit the file (in Nano, press Ctrl + X, then Y, and Enter).

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

__To see the version of Compiler:__

```
arm-unknown-linux-gnueabi-gcc --version
```
# 🔧 Chapter 4: The ToolChain sysroot (Info)

`Lets dive into the sysroot where all of your tools are present`

To find that place enter:
```
arm-unknown-linux-gnueabi-gcc -print-sysroot
```
get into the Library using `cd` 

you find this structure
```
.
├── etc
├── lib
├── sbin
├── usr   
     ├── bin
     ├── include
     ├── lib
     ├── libexec
     ├── sbin
     └── share
└── var
```

- `lib: Stores the shared objects for the C library and the dynamic linker/loader, including ld-linux.`

- `sbin: Provides the ldconfig utility, which optimizes library loading paths.`

**inside usr/**:

- `usr/lib: Contains the static library archive files for the C library and any other libraries that may be installed later.`


- `usr/include: Holds the headers for all libraries.`


- `usr/bin: Houses utility programs that run on the target system, such as the ldd command.`


- `usr/share: Used for localization and internationalization.`

# 📚  Chapter 5: Components of the C library
The C library is not a single file but is made up of four main components that together implement the POSIX API:

- **libc**: The core C library that includes familiar POSIX functions like `printf`, `open`, `close`, `read`, `write`, and more.
- **libm**: Contains mathematical functions such as `cos`, `exp`, and `log`.
- **libpthread**: Provides all POSIX thread functions, identified by names starting with `pthread_`.
- **librt**: Includes real-time extensions to POSIX, such as shared memory and asynchronous I/O.

The libc is default used while compilation. But lets say if we inclued `<math.h>` to use `log()` in our code then we need to add few more letters in out commad line:

```
arm-unknown-linux-gnueabi-gcc Some_mathfunction.c -o Some_mathfunction -lm
```
As the ``<math.h>`` is linked with `libm`. So, we need to mention with `-lm` at the end.

Lets analyze our compiled program. To do that run:
```
arm-unknown-linux-gnueabi-readelf -a Some_mathfunction 
```

To see insight of library run ( Shared Library ):
```
arm-unknown-linux-gnueabi-readelf -a Some_mathfunction | grep "Shared library"
```

Nice, you will find:
 - 0x00000001 (NEEDED)                     Shared library: **[libm.so.6]**
 - 0x00000001 (NEEDED)                     Shared library: **[libc.so.6]**
