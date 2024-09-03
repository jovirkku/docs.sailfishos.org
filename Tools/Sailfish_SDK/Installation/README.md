---
title: Installation
permalink: Tools/Sailfish_SDK/Installation/
parent: Sailfish SDK
layout: default
nav_order: 100
---

We provide a graphical installer to make it easy to setup the Sailfish SDK.

## Supported Environments

Sailfish SDK functionality has been verified on the following systems:

  - Ubuntu 22.04 64 bit
  - Windows 10 64 bit
  - macOS 13.6.7
    - Only Intel CPUs are supported. Apple silicon is not supported.

Sailfish SDK should work on other Linux flavours as well, but at this stage, its functionality on other host environments has not been fully verified.

## Pre-requisites

### Virtualization Platform

The native platform for Sailfish OS software development is GNU/Linux (Linux). In order to enable development on macOS and Windows as well as to support miscellaneous Linux variants more easily, Sailfish SDK encapsulates its back-end in a virtual machine running a specifically tailored Linux distribution, the so called SDK Build Engine.

In addition to that, Sailfish SDK provides virtualized runtime environment for testing Sailfish OS applications - the Sailfish OS Emulator.

On Linux and Windows, both Oracle VM VirtualBox (version 4.1.18 or higher) and Docker (version 18.09.3 or higher) are supported by the SDK Build Engine. On macOS, only VirtualBox is supported.

  - Oracle VM VirtualBox\
    ➕ Fully fledged solution\
    ➖ Less optimal build performance
  - Docker\
    ➕ Optimal build performance\
    ➖ Technical limitations affect some [configurations and use cases](/Tools/Sailfish_SDK/FAQ/#docker)

For Sailfish OS Emulator, Oracle VM VirtualBox is needed regardless of the host platform.

On Linux, you should install the virtualization platform packages supplied by your distribution. For example, on Ubuntu installing Docker happens simply with command `apt install docker.io`.
On other operating systems, we recommend using installation packages from <http://www.virtualbox.org> or <https://docs.docker.com/get-docker/>.

A few hints on Docker installation:

  - Follow the [generic instructions](https://docs.docker.com/get-docker/)
  - Linux users may need [extra steps](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user) to ensure that Sailfish SDK can manage Docker as a non-root user
    - Do *not* follow the instructions for installing Docker in rootless mode! Rootless mode is not needed for managing Docker as a non-root user.
  - Windows users should ensure that Docker is set up to use [Linux containers](https://docs.docker.com/desktop/windows/wsl/) (instead of Windows containers)
  - Using Docker in rootless mode is not supported

### MSYS2 (Windows only)

MSYS2 is not just the only one fully supported environment in case you plan command line use of Sailfish SDK on Windows. It also satisfies certain dependencies of the Sailfish IDE.

Follow the general installation instructions on <https://www.msys2.org/>, then install the tools required by Sailfish SDK by issuing the following command under the shell invoked with the "MSYS2 MSYS" Start menu item:

    pacman -S --needed base-devel git

You may also consider configuring MSYS2 to use your Windows user home directory. Otherwise it will use a directory located under MSYS2 installation directory. This can be changed by editing `/etc/nsswitch.conf` under MSYS2 shell, applying the following configuration:

    db_home: windows cygwin desc

When Docker is used, ensure that the `docker` command line tool is available on `PATH` under the MSYS2 shell. This can be accomplished by adding, e.g.,

    export PATH="$PATH:/c/Program Files/Docker/Docker/resources/bin/"
    
to the `~/.bash_profile` script under the MSYS2 shell and reopening the shell.

### System Requirements
  - About 15GB of free disk space for the default component selection
    - When Docker is used, installed files are split between the Sailfish SDK installation directory and Docker's [data-root](https://docs.docker.com/config/daemon/#docker-daemon-directory). Ensure that both provide enough free space.
  - 4GB of RAM or more is recommended.

### Others

#### libcrypt.so.1 (Linux only)

In order to maintain compatibility with older Linux distributions, Sailfish SDK links to this library, which may not be installed by default on more recent distributions:

  - On Arch Linux the package libxcrypt-compat must be installed

#### Modern bash

Command completion for sfdk, the command line frontend to the Sailfish SDK, is only provided for bash shell interpreter. This requirement concerns especially macOS users, where the default shell is either a too old bash or zsh. If you plan to use Sailfish SDK from CLI on macOS, it is recommended to use newer `bash` and `bash-completion@2` packages from [Homebrew](https://brew.sh).

See also [Sailfish SDK Known Issues](/Tools/Sailfish_SDK/Known_Issues).

## Sailfish SDK Installer

### SDK Installer on Linux

1.  [Download](/Tools/Sailfish_SDK#latest-sdk-release) the run file
2.  Open the Terminal application on your host system.
3.  Provide executable permissions to it: `$ chmod +x ~/Downloads/<installer_name>`
4.  Run the installer as a normal user (i.e. not as root user). By default it will install to `SailfishOS/` in your home directory.
    ```
    $ ./Downloads/<installer_name>
    ```

### SDK Installer on Windows

1.  [Download](/Tools/Sailfish_SDK#latest-sdk-release) SDK Installer application to your downloads folder and locate it with Explorer.
2.  Open SDK installer by double-clicking the SDK Installer application.

### SDK Installer on OS X

1.  [Download](/Tools/Sailfish_SDK#latest-sdk-release) SDK Installer package.
2.  Open SDK Installer package by double-clicking the icon.
3.  Open the SDK Installer application found from the package.

## Common installation flow

The SDK installer window will open as shown below. The installation itself is pretty simple.

<a href="Installer_01.png" style="width:30em;display:block">
    <img src="Installer_01.png"
         alt="Installer_01.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

Click **Next**, follow the setup wizard and read and accept the license agreement. The default values are usually OK. If you wish, you can see the details of the files being installed. After a successful installation you should see a window like the one shown below.

<a href="Installer_02.png" style="width:30em;display:block">
    <img src="Installer_02.png"
         alt="Installer_02.png"
         class="md_thumbnail" style="max-width:100%"/>
</a>

Click **Finish**. The Sailfish IDE launches automatically once the setup wizard exits.

## Launching the Sailfish IDE

  - The SDK will be added to your system menus and a launch icon will be available for future sessions.
  - On Linux you can launch the Sailfish IDE from desktop launcher by typing `Sailfish IDE` and choosing the appropriate icon. Alternatively, you can start it from command line by running `~/SailfishOS/bin/qtcreator`.
  - On Windows you can launch the Sailfish IDE by pressing Start and typing `Sailfish IDE` and choosing the appropriate application.
  - On OS X you can launch the Sailfish IDE by opening Launchpad and typing `Sailfish IDE`. (With Spotlight it can be found by typing `Qt Creator`.)

Once the SDK is installed, proceed to creating your [first application](/Tools/Sailfish_SDK/#guides)!

## Using the command line interface

[sfdk](/Develop/Apps/#sfdk-command-line-tool), the command line frontend to the Sailfish SDK, can be found under the `bin` subdirectory of the installation directory.

Adding SDK's `bin` directory to `PATH` is not recommended. Creating a shell wrapper under your `~/bin` directory is the most widely available option (provided that your `~/bin` directory is on `PATH`)

    cat >~/bin/sfdk <<'END'
    #!/bin/sh
    exec /path/to/SailfishOS/bin/sfdk "$@"
    END
    chmod +x ~/bin/sfdk

On Windows it may be necessary to omit the `exec` keyword. Depending on your Windows version, the following error may occur when the `exec` keyword is used:

    [D] SOFT ASSERT: "parentPidIt != parentPids.constEnd()" in file ...\session.cpp, line 279

On Linux (exclusively) you can also simply symlink `sfdk` into `~/bin` instead of creating a shell wrapper (again, provided that your `~/bin` directory is on `PATH`)

    ln -s /path/to/SailfishOS/bin/sfdk ~/bin/sfdk

Start by reading the built-in help.

    sfdk --help

## Unattended/headless installation

Invoke the Sailfish SDK installer application with the following arguments to avoid need for any interaction during the installation process.

    ./installer.run non-interactive=1 accept-licenses=1 build-engine-type={vbox|docker}

On Linux, the installer application may be run without graphical user interface too. Set the `QT_QPA_PLATFORM` environment variable to achieve this.

    QT_QPA_PLATFORM=minimal ./installer.run --verbose ...

When installed on a multiuser machine, change the TCP ports used by the build engine to avoid conflicts with other users.

    sfdk engine set ssh.port=<number> dbus.port=<number>
