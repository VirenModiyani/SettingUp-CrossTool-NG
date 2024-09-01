# SettingUp-CrossTool-NG
This Repository is about the command require &amp; Guide to setup the ToolChain in LINUX for Embedded Systems:

## Step1: Install The Required Library (you may require few more, If you get error you can ind & install missing one):

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

## Step 2: Bootstrap and Configure

Run the bootstrap script to set up the build environment:
```
./bootstrap
```
Next, configure the build system:

` The `--enable-local` option installs the program in the current directory, eliminating the need for root permissions. This is in contrast to the default installation path, `/usr/local/bin`, which would require elevated privileges.`
```
./configure --enable-local
```
## Step 3: Build and Install

Compile and install Crosstool-NG:
```
make
sudo make install
```

## Step 4: Verify the Installation

After installation, you can verify that Crosstool-NG is installed correctly by checking its version:
```
./ct-ng version
```
