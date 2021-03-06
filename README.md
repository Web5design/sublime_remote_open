Sublime Remote Open
===================

A Sublime Text 2/3 plugin that allow you to open files in a running sublime text instance from a remote machine.
Usually requires a shared directory of some kind.

If you have a machine on which you are running Sublime Text 2 or 3 and it shares common directories with another machine.
Such as a samba mounted drive or a network drive for instance.

If you get tired of working on the remote machine and then having to switch back to sublime and navigate to the files you want to open. You can use this plugin to open the files in the Sublime Text on the first machine with a command on the other machine.


## Alternatives

This is similar to the [rmate](http://canadian-fury.com/2012/06/26/using-textmate-2-s-rmate-with-sublime-text-2/) plugin. Except that *rmate* uses ssh to get the file you are working on. *Remote Open* relies on the remote directory structure being mounted locally. I think that the advantage of the local mounting allows you to take advantage of Sublime Text 3's Project Indexing because all files from a directory structure can be accessed by Sublime and indexed for quick navigation with the *Go To Symbol* short cuts.


## Installation

### Package Control

You can install this plugin using the fantastic Sublime Package Control Plugin.

You can find information and instructions on installing it here:
[http://wbond.net/sublime_packages/package_control](http://wbond.net/sublime_packages/package_control)

Once you have it installed.

Open the Command palette in your Sublime Text with:

On Linux and Windows:

    control + shift + p

On OSX:

    super + shift + p

From the palette, select:

    Package Control: Install Package

Wait for another palette to appear, which will list all available plugins, select:

    RemoteOpen


### Git

Clone this repository into your Sublime Text *Packages* directory.

    git clone https://github.com/sligodave/sublime_remote_open.git RemoteOpen


## Configuration

* Open the settings file and configure as needed.

    Packages/User/RemoteOpen.sublime-settings

To find the *Packages* directory, from the Command Palette, select:

    Preferences: Browse Packages

```
{
	// Whether or not the plugin prints information to the python command
	"debug": false,
	// The host at which the server should listen. E.g. 192.186.0.X
	"host": "localhost",
	// The port at which the server should listen
	"port": 25252,
	// Remote to local path mappings.
	// These are the locations on your remote machine and their corresponding
	// locations on your local machine.
	// I.e. Where you have mapped the remote locations too locally.
	"path_maps": {
		// "/REMOTE/PATH/BASE": "/LOCAL/PATH/BASE"
		// "/home/username/work": "G:/"
		// "/home/username_remote/work": "/home/username_local/maps/remote_home_dir/work"
	},
	// Should the server open all files in a directory when one is sent
	"open_directory_contents": true,
	// Should the server open all files recursively in a directories sub structure.
	// Requires "open_directories" to be
	"open_directory_recursively": true,
	// runs the listening server when sublime starts.
	"listen_on_startup": true
}	
```

_The above are the defaults._

* Place the contents of the *remote_machine* directory onto your remote machine.
* Place the contents into a location on your $PATH so it will be found.
* Ensure that the *subl_remote_open* file is executable.
* Open the *subl_remote_open|subl_remote_open.bat* file and append the *address* and *port* that you give your server above to the python command in this file.
* The *subl_remote_open|subl_remote_open.bat* file expects that "python" is available in your $PATH. If it isn't, update the file with the path to the python executable too.

For example (linux/osx but windows is similar):

    python ${DIR}/subl_remote_open.py localhost 25252 "$@"

becomes:

    python ${DIR}/subl_remote_open.py 192.168.0.X 25252 "$@"


## Usage

### Server

To start the server in your Sublime Text so that it will listen for remote requests.
From the *Go To Anything* Palette:

	Remote Open: Start Listening

To stop the server.
From the *Go To Anything* Palette:

	Remote Open: Stop Listening


*NOTE:*
You can also set the server to start automatically when Sublime Starts using the "listen_on_startup" setting.


### Client / Remote Open command

With Sublime Text open and the Remote Open server listening on your local machine.
Run the supplied *subl_remote_open* commmand on your remote machine, passing it a file to open.

    subl_remote_open FILE_TO_OPEN.EXT

The command and accompanying python will take care of the paths etc from here and
send the request to the server to open the file on the local machine.

You can specify a line number to open the file at also by using the ':' standard.
E.g.

    subl_remote_open FILE_TO_OPEN.EXT:45

Will open the file with the cursor on line 45.


## Troubleshooting

I've had not problems so far but I'll add things here as they come up.

Off hand, if you are having trouble connecting to the listening Sublime Text from your remote machine. Check to make sure that the firewall on your local machine where the Sublime Text is running allows the connection you are trying to make.

*Remote machine can no longer see server*

I did a major overhaul of the code to use a much saner threading server, in doing so I renamed the "address" setting to "host".
So you can fix this problem by updating your RemoteOpen.sublime-settings file.


## To Do

Nothing right now, send on suggestions.


## Issues / Suggestions:

Send on any suggestions or issues.


## Copyright and license
Copyright 2013 David Higgins

[MIT License](LICENSE)
