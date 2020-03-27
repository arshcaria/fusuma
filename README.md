# Fusuma [![Gem Version](https://badge.fury.io/rb/fusuma.svg)](https://badge.fury.io/rb/fusuma) [![Build Status](https://travis-ci.com/iberianpig/fusuma.svg?branch=master)](https://travis-ci.com/iberianpig/fusuma)

Fusuma is multitouch gesture recognizer.
This gem makes your linux able to recognize swipes or pinchs and assign commands to them.

![fusuma_image](https://i.gyazo.com/757fef526310b9d68f68e80eb1e4540f.png)

襖(Fusuma) means sliding door used to partition off rooms in a Japanese house.

## Installation

### 1. Grant permission to read the touchpad device
**IMPORTANT**: You **MUST** be a member of the **INPUT** group to read touchpad by Fusuma.

```bash
$ sudo gpasswd -a $USER input
```

Then, You **MUST** **REBOOT** to assign this group.

### 2. Install libinput-tools
You need `libinput` release 1.0 or later.

```bash
$ sudo apt-get install libinput-tools
```

### 3. Install Ruby
Fusuma runs in Ruby, so you must install it first.

```bash
$ sudo apt-get install ruby
```

### 4. Install Fusuma

```bash
$ sudo gem install fusuma
```

### 5. Install xdotool (optional)

For sending shortcuts:
```bash
$ sudo apt-get install xdotool
```

### Touchpad not working in GNOME

Ensure the touchpad events are being sent to the GNOME desktop by running the following command:


```bash
$ gsettings set org.gnome.desktop.peripherals.touchpad send-events enabled
```

## Usage

```bash
$ fusuma
```

## Update

```bash
$ sudo gem update fusuma
```

## Customize Gesture Mapping

You can customize the settings for gestures to put and edit `~/.config/fusuma/config.yml`.  
**NOTE: You will need to create the `~/.config/fusuma` directory if it doesn't exist yet.**

```bash
$ mkdir -p ~/.config/fusuma        # create config directory
$ nano ~/.config/fusuma/config.yml # edit config file.
```

### About YAML Basic Syntax
* Comments in YAML begins with the (#) character.
* Comments must be separated from other tokens by whitespaces.
* Indentation of whitespace is used to denote structure.
* Tabs are not included as indentation for YAML files.


### Example: Gesture Mapping for Ubuntu

https://github.com/iberianpig/fusuma/wiki/Ubuntu

```yaml
swipe:
  3: 
    left: 
      command: 'xdotool key alt+Right'
    right: 
      command: 'xdotool key alt+Left'
    up: 
      command: 'xdotool key super'
    down: 
      command: 'xdotool key super'
  4:
    left: 
      command: 'xdotool key ctrl+alt+Down'
    right: 
      command: 'xdotool key ctrl+alt+Up'
    up: 
      command: 'xdotool key ctrl+alt+Down'
    down: 
      command: 'xdotool key ctrl+alt+Up'
pinch:
  in:
    command: 'xdotool keydown ctrl click 4 keyup ctrl'
  out:
    command: 'xdotool keydown ctrl click 5 keyup ctrl'
```

### More Example of config.yml
The following wiki pages can be edited by everyone.

https://github.com/iberianpig/fusuma/wiki/

* [Ubuntu · iberianpig/fusuma Wiki](https://github.com/iberianpig/fusuma/wiki/Ubuntu)
* [elementary OS · iberianpig/fusuma Wiki](https://github.com/iberianpig/fusuma/wiki/elementary-OS)
* [i3 · iberianpig/fusuma Wiki](https://github.com/iberianpig/fusuma/wiki/i3)
* [KDE to mimic MacOS · iberianpig/fusuma Wiki](https://github.com/iberianpig/fusuma/wiki/KDE-to-mimic-MacOS)
* [POP OS with Cinnamon · iberianpig/fusuma Wiki](https://github.com/iberianpig/fusuma/wiki/POP-OS-with-Cinnamon)
* [PopOS Default Gnome · iberianpig/fusuma Wiki](https://github.com/iberianpig/fusuma/wiki/PopOS-Default-Gnome)
* [Ubuntu OS to mimic Mac a little · iberianpig/fusuma Wiki](https://github.com/iberianpig/fusuma/wiki/Ubuntu-OS-to-mimic-Mac-a-little)

If you have a nice configuration, please share `~/.config/fusuma/config.yml` with everyone.

### Threshold and Interval
if `command: ` properties are blank, the swipe/pinch doesn't execute command.

`threshold:` is sensitivity to swipe/pinch. Default value is 1.
If the swipe's threshold is `0.5`, shorten swipe-length by half.

`interval:` is delay between swipes/pinches. Default value is 1.
If the swipe's interval is `0.5`, shorten swipe-interval by half to recognize a next swipe.

### `command: ` property for assigning commands
On fusuma version 0.4 `command: ` property is available!
You can assign any command each gestures.

**`shortcut: ` property is deprecated**, **it was removed on fusuma version 1.0**.
You need to replace to `command: ` property.


```diff
swipe:
  3:
    left:
-      shortcut: 'alt+Left'
+      command: 'xdotool key alt+Left'
    right:
-      shortcut: 'alt+Right'
+      command: 'xdotool key alt+Right'
```

### About xdotool


* xdotool manual (https://github.com/jordansissel/xdotool/blob/master/xdotool.pod)
* Available keys' hint (https://github.com/jordansissel/xdotool/issues/212#issuecomment-406156157)

**NOTE: xdotool has some issues**

* Gestures take a few seconds to react(https://github.com/iberianpig/fusuma/issues/113)

#### Alternatives to xdotool

  * [fusuma-plugin-sendkey](https://github.com/iberianpig/fusuma-plugin-sendkey) 
    * Emulates keyboard events
    * Wayland compatible

  * `xte`
    * [xte(1) - Linux man page](https://linux.die.net/man/1/xte)
    * install with `sudo apt xautomation`

## Options

*   `-c`, `--config=path/to/file` : Use an alternative config file
*   `-d`, `--daemon`              : Daemonize process
*   `-l`, `--list-devices`        : List available devices
*   `-v`, `--verbose`             : Show details about the results of running fusuma
*   `--device="Device name"`      : Open the given device only
*   `--version`                   : Show fusuma version

## Autostart (gnome-session-properties)
1. Check the path where you installed fusuma with `$ which fusuma`
2. Open `$ gnome-session-properties`
3. Add Fusuma and enter the location where the above path was checked in the command input field
4. Add the `-d` option at the end of the command input field

## Fusuma Plugins

Following features are provided as plugins.

 * Adding new gestures or combinations
 * Features for specific Linux distributions
 * Setting different gestures per applications

### Installation of fusuma plugins

Fusuma plugins are provided with the `fusuma-plugin-XXXXX` naming convention and hosted on [RubyGems](https://rubygems.org/search?utf8=%E2%9C%93&query=fusuma-plugins).

`$ sudo gem install fusuma-plugin-XXXXX`

### Available plugins

| Name                                                                           | About                                         |
|--------------------------------------------------------------------------------|-----------------------------------------------|
| [fusuma-plugin-sendkey](https://github.com/iberianpig/fusuma-plugin-sendkey)   | Emulates keyboard events                      |
| [fusuma-plugin-wmctrl](https://github.com/iberianpig/fusuma-plugin-wmctrl)     | Manages Window and Workspace                  |
| [fusuma-plugin-keypress](https://github.com/iberianpig/fusuma-plugin-keypress) | Detects gestures while pressing multiple keys |
| [fusuma-plugin-tap](https://github.com/iberianpig/fusuma-plugin-tap)           | Detects Tap and Hold gestures                 |

## Tutorial Video

[![Multitouch Touchpad Gestures in Linux with Fusuma](http://img.youtube.com/vi/bn11Iwvf29I/0.jpg)](http://www.youtube.com/watch?v=bn11Iwvf29I "Multitouch Touchpad Gestures in Linux with Fusuma")   
[Multitouch Touchpad Gestures in Linux with Fusuma](http://www.youtube.com/watch?v=bn11Iwvf29I) by [Eric Adams](https://www.youtube.com/user/igster75)  


## Support

I'm a Freelance Engineer in Japan and working on these products after finishing my regular work or on my holidays.
Currently, my open-source contribution times is not enough.
If you like my work and want to contribute and become a sponsor, I will be able to focus on my projects.

* [GitHub Sponsors](https://github.com/sponsors/iberianpig) (Zero fee!)
* [Patreon](https://www.patreon.com/iberianpig)


## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/iberianpig/fusuma. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

