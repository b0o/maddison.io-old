---
date: 2022-01-20T03:53
---

# Multiple windows for a single Obsidian vault

Obsidian, the magical productivity tool that has capture my heart, has only one flaw in my eyes: it lacks the ability to create multiple windows for a single vault! Let's fix that…

Here's how I accomplished this on Linux (Arch btw) along with Obsidian sync.

#### Configuring a separate instance of Obsidian

1. Make a new directory inside your `XDG_CONFIG_HOME`, call it whatever you want. I called mine `obsidian-scratch` because I want to use the second instance as a scratchpad. The full path to this directory is `~/.config/obsidian-scratch` for me. 

   *(I'm going to use this `obsidian-scratch` name through the following steps, but feel free to make it your own and call it anything you want, just remember to update any commands below with your specifics)*

2. `cd` into this directory and create a few symbolic links. It should probably be fine if any of them fail due to a file not existing, don't sweat.

	```plain
	$ cd ~/.config/obsidian-scratch
	$ ln -s ../Electron .
	$ ln -s ../gtk-3.0 .
	$ ln -s ../gtkrc .
	$ ln -s ../electron-flags.conf .
	```

3. Create a new vault next to your target vault (not nested inside). For reference, I keep all of my vaults inside `~/Documents/obsidian`:

	```plain
	$ cd ~/Documents/obsidian
        $ ls -1
        maddy
        super-secret-vault
        work
	```

   The vault I'm interested in is `maddy`, so I'm going to make a vault named `maddy-scratch`.

	```plain
        $ mkdir maddy-scratch
        $ ls -1
        maddy
        maddy-scratch
        super-secret-vault
        work
	```

    Now the fun part: Make sure your current vault is all sync'd up, then open a new instance of Obsidian, overriding `XDG_CONFIG_HOME` with our new config directory. Keep your existing instance of Obsidian running so we can verify everything's working.

	```plain
        $ XDG_CONFIG_HOME="$XDG_CONFIG_HOME/obsidian-scratch" obsidian
	```
   
     When you're prompted to select a folder or create a vault, select the new `obsidian-scratch` directory and let 'obby do its magic. Once the Obsidian window opens up, go to your settings, log in, and set it up to sync with your target vault. You'll most likely want to turn on *all* of the sync options so everything stays `:chefs-kiss:`. 

   Give it some time to finish, then if desired go and enable community plugins so that stuff gets successfully sync'd as well. You can check the sync activity logs in the sync settings and once you get the all-clear message of `Fully synced`, close your new instance of Obsidian, then re-open it with that same weird `XDG_CONFIG_HOME ....` command from earlier.

   Now you should be able to use two independent instances of Obsidian with no resource fighting! You just need to launch the second instance using the above command. Good on you. 

4. The final step is to set up an easier way to launch your secondary instance. I personally use a binding in my window manager that opens a scratchpad with this instance, but you could just as easily create a new `obsidian-scratch.desktop` file. I'll walk you through that in case you're interested, see below.


---
#### Creating a desktop entry

   I. Your desktop file directory should be at `~/.local/share/applications`. If there's an `obsidian.desktop` file in there, copy it to `obsidian-scratch.desktop` in the same directory. Otherwise, you'll probably find the vanilla obsidian desktop file at `/usr/share/applications/obsidian.desktop`, and if so copy *that* one to your user desktop folder at `~/.local/share/applications/obsidian-scratch.desktop`. 

   II. Since you're cool, you'll probably want to edit this new file in vim, so go ahead and change the `Exec=` line to look like this: `Exec=/bin/sh -c 'env XDG_CONFIG_HOME="$XDG_CONFIG_HOME/obsidian-scratch" obsidian %U'`. Then change the name as you desire, like `Name=Obsidian (Scratch)`

   III. Verify it worked by opening your launched (rofi or whatever you use) and searching for obsidian, you should see both the vanilla desktop entry and your new, cool "Obsidian (Scratch)" entry. You should also be able to launch it from the command line with `xdg-launch obsidian-scratch`.

   IV. Go now, you do you.
