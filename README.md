This is a tool for [Factorio](http://www.factorio.com/) headless server management.
It provides mod management : List / Install / Update / Remove / Enable / Disable.

This package is heavily inspired by [Factorio-mod-updater](https://github.com/astevens/factorio-mod-updater/blob/master/factorio-mod-updater) and [Factorio Updater](https://github.com/narc0tiq/factorio-updater) so big think to you for your inspiration !

## Installation ##

Just clone this repository in any directory. Here, `/opt/factorio-mod-manager` as an example.
```bash
git clone git@github.com:Tantriss/Factorio-mod-manager.git /opt/factorio-mod-manager
```

This script has been tested (only on Debian) with Python 2.7 and 3.5 using a single non-standard library, [Requests](http://requests.readthedocs.org/en/latest/).

To install the required dependency, you should need do no more than run `pip
install requests` (or, `easy_install requests`). If this
does not work, you are encouraged to read the linked documentation and try to
figure out what's gone wrong.

## Configuration ##

Some constant parameters can be put in a config file. These options are :

* verbose (enable verbose mode)
* factorio_path (path to the root folder of your Factorio installation)
* username (Your username, see [Username and token](#username-and-token))
* token (Your token, see [Username and token](#username-and-token))
* should_reload (If the script should try to reload Factorio via systemctl and the service_name parameter)
* service_name (If Factorio is started via a service and you want to restart it automatically otherwise let "null")

An example file can be found in this repo, just copy `config.example.json` to `config.json` and edit values inside.

Keep in mind that any corresponding command line argument will override these in config file.

## Usage ##

From there, it's really simple: go in the folder you where you cloned this repo earlier, and run it (try it with `--help` first!). Here's an example session:

```bash
$ python mods_manager.py -h
usage: mods_manager.py [-h] [-p FACTORIO_PATH] [-u USERNAME] [-t TOKEN] [-d]
                       [-i MOD_NAME_TO_INSTALL] [-U] [-e] [-l]
                       [-r REMOVE_MOD_NAME] [-E LIST_ENABLE_MODS]
                       [-D LIST_DISABLE_MODS] [-v]

Install / Update / Remove mods for Factorio

optional arguments:
  -h, --help            show this help message and exit
  -p FACTORIO_PATH, --path-to-factorio FACTORIO_PATH
                        Path to your Factorio folder.
  -u USERNAME, --user USERNAME
                        Your Factorio service username, from player-data.json.
  -t TOKEN, --token TOKEN
                        Your Factorio service token, from player-data.json.
  -d, --dry-run         Don't download files, just state which mods updates
                        would be downloaded.
  -i MOD_NAME_TO_INSTALL, --install MOD_NAME_TO_INSTALL
                        Install the given mod. See README to find how easily
                        get the correct name for the mod.
  -U, --update          Enable the update process. By default, all mods are
                        updated. Seed -e/--update-enabled-only.
  -e, --update-enabled-only
                        Will only updates mods 'enabled' in 'mod-list.json'.
  -l, --list            List installed mods and return. Ignore other switches.
  -r REMOVE_MOD_NAME, --remove REMOVE_MOD_NAME
                        Remove specified mod.
  -E LIST_ENABLE_MODS, --enable-mod LIST_ENABLE_MODS
                        A mod name to enable. Repeat the flag for each mod you
                        want to enable.
  -D LIST_DISABLE_MODS, --disable-mod LIST_DISABLE_MODS
                        A mod name to disable. Repeat the flag for each mod
                        you want to disable.
  -v, --verbose         Print URLs and stuff as they happen.

```

--------

### More complex example :

Here we want to :

* Install "bobvehicleequipment"
* Enable "bobplates" and "bobgreenhouse" (assuming they are already installed)
* Disable "IndustrialRevolution" because of incompatibility issues with bob's mods
* Finally we update all mods

All this can be done in one command :
```bash
$ python mods_manager.py -p /app/projects/factorio/factorio -u YOUR_USER -t YOUR_TOKEN -i bobvehicleequipment -E bobplates -E bobgreenhouse -D IndustrialRevolution -U
Loading configuration...
Auto-detected Factorio version 0.17 from binary.
Enabling mod(s) ['bobplates', 'bobgreenhouse']
Parsing "mod-list.json"...
Writing to mod-list.json

Disabling mod(s) ['IndustrialRevolution']
Parsing "mod-list.json"...
Writing to mod-list.json

Starting mods update...
Parsing "mod-list.json"...
Getting mod "bobinserters" infos...
A file already exists at the path "/app/projects/factorio/factorio/mods/bobinserters_0.17.10.zip" and is identical (same SHA1), skipping...
Getting mod "bobplates" infos...
Downloading mod bobplates
[==================================================]

Installing mod bobvehicleequipment
Getting mod "bobvehicleequipment" infos...
Parsing "mod-list.json"...
Writing to mod-list.json
Downloading mod bobvehicleequipment
[==================================================]

Finished !

```

## Username and token ##

The keen-eyed will have noticed the options for `--user` and `--token`. These
allow you to supply a username and token normally used by the Factorio (like the in-game updater and authenticated multiplayer). Having them will
allow you to download the mods from the [Factorio API](https://mods.factorio.com/api/mods).

First, how to get them:
* Go to [the Factorio website](https://www.factorio.com/login) and login to your account.
* Click your username in top right to go to your profile.

## How to find the correct mod name

In order to use this script, you have to find the correct mod name, not the "friendly" one.
You can do it directly from mod portal !

Once you find an interesting mod, for example `Bob's Metals, Chemicals and Intermediates`, open the mod portal page, here https://mods.factorio.com/mod/bobplates

The correct mod name to use is the last part of the URL : `bobplates`.

## License ##

The source of **Factorio Mod Manager** is Copyright 2019 Tristan "Tantriss"
Chanove. It is licensed under the [MIT license][mit], available in this
package in the file [LICENSE.md](LICENSE.md).

[mit]: http://opensource.org/licenses/mit-license.html


## TODO ##
- Add crontab example
- Interactive mod
- Support multiple instances of Factorio
