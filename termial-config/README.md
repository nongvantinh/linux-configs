GNOME Terminal itself doesn't provide such an option, so you have to manually operate on its database.

Beginning with version 3.8 it uses GSettings, which in turn (at least on Linux systems) uses dconf. It would probably be more elegant to go with the gsettings tool. Unfortunately I couldn't figure out how to dump all the relevant data there, let alone restore them. So let's use dconf.


**First install dconf-editor***

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

## You can export the list of keybindings and import them whenever needed.

```

$ dconf list /org/gnome/terminal/legacy/keybindings/
$ dconf read /org/gnome/terminal/legacy/keybindings/new-tab

$ dconf dump /org/gnome/terminal/legacy/keybindings/ > keys.dconf

```

To import the custom keybinding run the following load command.

```

dconf load /org/gnome/terminal/legacy/keybindings/ < keys.dconf

```

## How to Export and Import Gnome Terminal Profile only

When you customize your terminal settings you will name them or be it a default profile. I have a profile created named “Ubuntu1”. Each profile will have a UID associated with it. Run the following command to get the list of profiles.

```

$ dconf list /org/gnome/terminal/legacy/profiles:/

```

```

output:
	:b1dcc9dd-5262-4d8d-a863-c897e6d979b9/
	:963a71e9-42c9-416e-b261-bcc4c6c70b36/
	:abcb9ad5-2b80-4ea0-a98a-22679d4506f0/
	:f5ebf9b6-7851-4772-8d00-3ed37591885f/
	default
	list
```

You can see there are UID that are part of the profiles I created. You can cross-verify it from your terminal profile. Exporting the profile is the same as what we did for keybindings.

We are going to use the dump command to export the changes and the load command to import the profile. Run the following command to export the profile. Replace UID with your profile UID.

```

$ dconf dump /org/gnome/terminal/legacy/profiles:/<UUID> > <name>.dconf

```

e.g:
$ dconf dump /org/gnome/terminal/legacy/profiles:/:b1dcc9dd-5262-4d8d-a863-c897e6d979b9/ > ubuntu1.dconf

Now reset your terminal settings and import the profile to see if everything works fine.

```

$ dconf reset -f /org/gnome/terminal/legacy/profiles:/
$ dconf load /org/gnome/terminal/legacy/profiles:/:b1dcc9dd-5262-4d8d-a863-c897e6d979b9/ < ubuntu1.dconf 

```

References:
https://www.linuxshelltips.com/export-import-gnome-terminal-profile/
https://askubuntu.com/questions/967517/backup-gnome-terminal
