# RISC-V Development Environment Setup on WSL

This guide provides step-by-step instructions for installing Windows Subsystem for Linux (WSL) and setting up the necessary tools for RISC-V development, including:
- GNU toolchain for RISC-V (commit hash `a33dac0`)
- RISC-V ISA simulator (Spike; commit hash `bbaec51`)
- Proxy kernel (commit hash `e5563d1`)

First, we have to download WSL on your Windows system:

## Step 1: Install WSL
1. Open PowerShell as Administrator and run:
   ```powershell
   wsl --install
   ```
2. Restart your computer if prompted.
   After rebooting, open PowerShell as Administrator again, and run following command to download Ubuntu.
   ```
   wsl --install -d Ubuntu
   ```
5. Do a checking of the system:
   Use the following command to check the version of the valid WSL and Ubuntu
   ```sh
   wsl --list
   ```
7. Activate Ubuntu environment
   ```
   wsl -d Ubuntu
   ```

## Step 2: Install GNU Toolchain for RISC-V (GCC tools)
Create a package folder under the /opt folder (i.e., /opt/riscv ). Subsequently, add the /opt/riscv to the
environment variable $RISCV . Afterwards, create a sub-directory named bin within your package directory
(i.e., /opt/riscv/bin ). Finally, add the /opt/riscv/bin to the environment variable $PATH . The relevant
commands are provided below.
```sh
$ sudo mkdir /opt/riscv && sudo mkdir /opt/riscv/bin
$ echo 'export RISCV=/opt/riscv' >> ~/.bashrc
$ echo 'export PATH=$PATH:$RISCV/bin' >> ~/.bashrc
$ source ~/.bashrc
```

After that, you should create a project directory (e.g., $HOME/riscv ).
```sh
$ mkdir ~/riscv
```

## Step 3: Install GNU Toolchain for RISC-V

Run the following commands to Install the tool:
Retrieve and install the necessary packages using the sudo apt install command.
Clone the latest version of the riscv-gnu-toolchain project repository from GitHub using the git
clone command. Then fetches and checks out the correct versions of all submodules to match the state 
specified in the riscv-gnu-toolchain repository.

Execute the ./configure command to set up the environment variables before building the project.
To construct and install the project in parallel, utilize the sudo make linux -j$(nproc) command. This
command will utilize the number of logical threads available on your machine to execute the build process
concurrently. Note that if fatal error occured, try to decrese the number of parallel compilation.


```sh
$ cd ~/riscv
$ sudo apt update
$ sudo apt install autoconf automake autotools-dev curl python3 python3-pip libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev ninja-build git cmake libglib2.0-dev
$ git clone https://github.com/riscv/riscv-gnu-toolchain ~/riscv/riscv-gnu-toolchain
$ cd ~/riscv/riscv-gnu-toolchain
$ git submodule update --init --recursive
$ ./configure --prefix=$RISCV --enable-multilib
$ sudo make linux -j$(nproc) ## Use -j2 if fatal error occured
```
After the above operations, the riscv-gnu-toolchain will be installed under the path: $RISCV/riscv-gnutoolchain and the riscv64-unknown-linux-gnu packages will be installed under the path: $RISCV .
The following command is used to show the version information of the installed GCC compiler (the expected
output message is shown below.)

```sh
$ riscv64-unknown-linux-gnu-gcc -v
riscv64-unknown-linux-gnu-gcc (g04696df09) 14.2.0
```

## Step 4: Install RISC-V ISA Simulator (Spike)

## Step 5: Install Proxy Kernel

## Step 6: Verify Installation
Check if the toolchain, Spike, and proxy kernel are installed correctly:

If all commands execute successfully, your RISC-V development environment is ready!

