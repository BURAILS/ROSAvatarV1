

# Non-debian ROS Install
### This applies to Fedora, CentOS, Arch, etc.

Fire up your favorite terminal emulator (I use terminator with tmux because I never managed to configure rxvt right) and pull up a track from [electronic gems](https://www.youtube.com/watch?v=8GW6sLrK40k). We'll now be using Robotics Operating System (ROS). It's unlikely we'll encounter anything that requires the cutting-edge latest version of ROS, so we'll be installing "ROS-Melodic" instead of ROS2. Eventually we might migrate to ROS2 but thusfar it appears that it can only be compiled from source (scratch) and pretty much has no active support. 

If you install an earlier version of ROS, like ROS-Kinetic, it ought to work. But don't quote me on that...

It also seems like ROS has official support for _only_ Windows and Ubuntu. Pretty typical, considering their users probably use Windows >95% of the time. For the rest of us, like Mac users and every-other-linux-distro users (btw i use arch), the install is usually a hit or miss. 

Your default package manager like homebrew will probably have ROS/ROS dependencies. Relatively more obscure package managers/loaders, like snap and flatpak, will almost certainly not have it. 

1. **Installing**
  
If you're running Arch, the package is in AUR. I use `yay`:  

```
yay ros-melodic-desktop // Be sure *not* to select 'ros-melodic-desktop-full'

```
If you have a RedHat distro, like Fedora (my first distro!), you're not actually supported. Sorry. But you can certainly install a ton of dependences via `dnf` or `yum` and cobble together something that gets you by. Pray to Gaben and Torvalds that it works: 

```
dnf install gcc-c++ python-rosdep python3-rosinstall_generator python-wstool python-rosinstall @buildsys-build

```

If you have something based on CentOS or anything else meta-generic, `pip` actually gets you pretty far. Just make sure that you've installed C compiling libraries beforehand. Try to get Python 2.7 (3.8 throws issues left and right, not too surprising): 

```
pip install -U rosdep rosinstall_generator wstool rosinstall

```

Note that I omitted Debian. That's because Debian is Ubuntu in a blue disguise. Change my mind. 

Also, `pip` is common to pretty much all linux distros. In theory, you should be able to run the command immediately above on any distro, even Ubuntu, and install ROS. I haven't tried it, so don't quote me on that either.  

1. **Troubleshooting the install**

It's almost certain that you'll get errors on the first install attempt. This is usually because of Python. The most common message always has something to do with missing python.h. Check your error message to confirm that it is indeed a Python issue, because if you try to fix what ain't broke, Murphy and Parkinson's Law will eat you alive. Either pip (used to install minor dependencies at some point) or Python itself is out of date, missing, or corrupted. This can happen really easily if you don't edit Python scripts regularly. I really don't know why since Python is supposed to be the 'easy' language..

Fix it by updating `pip`:

```
sudo pip install --upgrade setuptools

```

You might also need an earlier version of Python. Sometimes Python38 works, sometimes it doesn't. So try uninstalling your current python libraries and reinstalling them. 

For Arch:

```
pacman -Rns *38 //refer to any package with a python38 dependency

```

For CentOS: 

```
dnf remove *38 //again, refer to any package with a python38 dependency

```

For any other distro, try `locate python38`. It's almost guaranteed that bash will say something like `command not found` or `python38 does not exist!`, but you get the point. Try to locate anything that's not python 2.8, and purge it from your system. Figure it out. 

If you're on Arch, there's a good chance that AUR has something like Python-2.7....:
```
yay python-2.7 

```

All other distributions have some variation on "python developer" that include 2.7 binaries. The thing is that some of them include Python 3.8 as well. Can't guarantee it won't brick your system, but installing several different versions of Python on your system will be less "bad" than half a version of Python:

```
dnf install python-devel

```

Debian/Ubuntu/EasyLinux 

```
apt-get install python-dev

```

If you get any other issues (unsupported C compiler, dependencies breaking) try to solve them and then let us know if it persists. 

1. **Initialize rosdep**

If everything works, yay! We're ready to do the actual setup. 

Start by initializing rosdep. It's yet another package manager, but this time for ROS:

```
rosdep init
rosdep update

```

Add the ROS Environment variables to BASH:

```
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc

```

And that's the setup! ROS should be functioning on your machine. 

To quickly verify that you've installed ros, try running ROSCD and ROSRUN in the terminal. If you get anything other than `command not found`, you've successfully installed it. Otherwise, let us know and we'll help!

Next up will be the tutorials (TBD).