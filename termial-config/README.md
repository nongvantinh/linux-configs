GNOME Terminal itself doesn't provide any backup data option, so you have to manually operate on its database.

Beginning with version 3.8 it uses GSettings, which in turn (at least on Linux systems) uses dconf. It would probably be more elegant to go with the gsettings tool. Unfortunately I couldn't figure out how to dump all the relevant data there, let alone restore them. So let's use dconf.


**First install dconf-editor**

```
sudo apt update
sudo apt install dconf-editor
```

# Export the whole settings of terminal.

To export terminal settings we simple dump it into a file.
```
dconf dump /org/gnome/terminal/ > gnome_terminal_settings_backup.txt
```
Reset (wipe out) the settings before loading a new one (probably not really required):
```
dconf reset -f /org/gnome/terminal/
```
Load the saved settings:
```
dconf load /org/gnome/terminal/ < gnome_terminal_settings_backup.txt
```
Disclaimer: I haven't tested the restore steps. I recommend that before the reset/load operations you back up your entire dconf database, which is stored in the single file ~/.config/dconf/user, using a simple standard filesystem copy operation (as opposed to some dconf command). In case of problem you can restore it just as easily (maybe from another terminal emulator or the Linux console).


# How to Export Gnome Terminal Keybindings only

To export terminal keybindings we simple dump it into a file.
```
dconf dump /org/gnome/terminal/legacy/keybindings/ > keybindings_backup.txt
```
Reset (wipe out) the settings before loading a new one (probably not really required):
```
dconf reset -f /org/gnome/terminal/legacy/keybindings/
```
Load the saved settings:
```
dconf load /org/gnome/terminal/legacy/keybindings/ < keybindings_backup.txt
```

Now reset your terminal settings and import the profile to see if everything works fine.

## How to Export and Import Gnome Terminal Profile only

When you customize your terminal settings you will name them or be it a default profile. I have a profile created named “Ubuntu1”. Each profile will have a UID associated with it. Run the following command to get the list of profiles.

To export terminal profiles settings we simple dump it into a file.
```
dconf dump /org/gnome/terminal/legacy/profiles:/ > profiles_backup.txt
```
Reset (wipe out) the settings before loading a new one (probably not really required):
```
dconf reset -f /org/gnome/terminal/legacy/profiles:/
```
Load the saved settings:
```
dconf load /org/gnome/terminal/legacy/profiles:/ < profiles_backup.txt
```

Now reset your terminal settings and import the profile to see if everything works fine.


References:
https://www.linuxshelltips.com/export-import-gnome-terminal-profile/
https://askubuntu.com/questions/967517/backup-gnome-terminal
