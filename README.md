# Nostalgia bucklespring keyboard sound

Copyright 2016 Ico Doornekamp

This project emulates the sound of my old faithful IBM Model-M space saver
bucklespring keyboard while typing on my notebook, mainly for the purpose of
annoying the hell out of my coworkers.

![Model M](img/model-m.jpg)
![Buckle](img/buckle.gif)

Bucklespring runs as a background process and plays back the sound of each key
pressed and released on your keyboard, just as if you were using an IBM
Model-M. The sound of each key has carefully been sampled, and is played back
while simulating the proper distance and direction for a realistic 3D sound
palette of pure nostalgic bliss.

To temporarily silence bucklespring, for example to enter secrets, press
ScrollLock twice (but be aware that those ScrollLock events _are_ delivered to
the application); same to unmute. The keycode for muting can be changed with
the `-m` option. Use keycode 0 to disable the mute function.

## Installation

[![Packaging status](https://repology.org/badge/tiny-repos/bucklespring.svg)](https://repology.org/project/bucklespring/versions)

### Debian

Bucklespring is available in the latest Debian and Ubuntu dev-releases, so you can
install with

```
$ sudo apt-get install bucklespring
```

### VoidLinux

Bucklespring is available in the VoidLinux repositories, so you can install with

```
$ sudo xbps-install -S bucklespring
```

### FreeBSD

Bucklespring can be installed via package:

```
$ pkg install bucklespring
```

or built via port:

```
$ cd /usr/ports/games/bucklespring
$ make install clean
```

### Linux, building from source

To compile on debian-based linux distributions, first make sure the require
libraries and header files are installed, then simply run `make`:

#### Dependencies on Debian

```
$ sudo apt-get install libopenal-dev libalure-dev libxtst-dev pkg-config
```

#### Dependencies on Arch Linux

```
$ sudo pacman -S openal alure libxtst
```

#### Dependencies on Fedora Linux

```
$ sudo dnf install gcc openal-soft-devel alure-devel libX11-devel libXtst-devel
```

#### Building

```
$ make
$ ./buckle
```

The default Linux build requires X11 for grabbing events. If you want to use
Bucklespring on the linux console or Wayland display server, you can configure
buckle to read events from the raw input devices in /dev/input. This will
require special permissions for buckle to open the devices, though. Build with

```
$ make libinput=1
```

#### Using snap on Ubuntu (since 16.04) and other distros

```
$ sudo snap install bucklespring
$ bucklespring.buckle
```

The snap includes the OpenAL configuration tweaks mentioned in this README.
See http://snapcraft.io/ for more info about Snap packages

### MacOS

Bucklespring builds and runs on macOS without any external dependencies. The ALURE library dependency has been removed and replaced with a built-in WAV file loader.

```
$ git clone https://github.com/zevv/bucklespring.git && cd bucklespring
$ make
$ ./buckle
```

**System Requirements:**

- macOS with built-in OpenAL framework (included since macOS 10.4)
- No additional package installation required

**Permissions:**

- You need superuser privileges to create the event tap on macOS
- Grant your terminal Accessibility rights: System Preferences → Security & Privacy → Accessibility

**Usage:**

```
$ sudo ./buckle
```

If you want to use buckle while doing normal work, add an & behind the command:

```
$ sudo ./buckle &
```

**Note:** The build may show deprecation warnings for OpenAL functions since macOS 10.15, as Apple recommends AVAudioEngine for new applications. However, OpenAL continues to work perfectly for existing applications like bucklespring.

### Windows

I think the windows build is currently broken, it seems that switching from
Freelut to Alure broke windows, I might fix this one day.

I suspect there is something wrong with `alureCreateBufferFromFile()` getting
called from another thread in the key capture callback, but my knowledge of the
win32 platform is so poor I'm not even able to run a debugger to see what is
happening. Help from an expert is much appreciated.

## Usage

```
usage: ./buckle [options]

options:

  -d DEVICE use OpenAL audio device DEVICE
  -f        use a fallback sound for unknown keys
  -g GAIN   set playback gain [0..100]
  -m CODE   use CODE as mute key (default 0x46 for scroll lock)
  -M        start the program muted
  -h        show help
  -l        list available openAL audio devices
  -p PATH   load .wav files from directory PATH
  -s WIDTH  set stereo width [0..100]
  -v        increase verbosity / debugging
```

## OpenAL notes

Bucklespring uses the OpenAL library for mixing samples and providing a
realistic 3D audio playback. This section contains some tips and tricks for
properly tuning OpenAL for bucklespring.

- The default OpenAL settings can cause a slight delay in playback. Edit or create
  the OpenAL configuration file `~/.alsoftrc` and add the following options:

```
period_size = 32
periods = 4
```

- If you are using headphones, enabling the head-related-transfer functions in OpenAL
  for a better 3D sound:

```
hrtf = true
```

- When starting an OpenAL application, the internal sound card is selected for output,
  and you might not be able to change the device using pavucontrol. The option to select
  an alternate device is present, but choosing the device has no effect. To solve this,
  add the following option to the OpenAL configuration file:

```
allow-moves = true
```
