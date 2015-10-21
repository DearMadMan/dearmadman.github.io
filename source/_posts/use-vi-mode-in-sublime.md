title: use vi mode in sublime
date: 2015-10-15 10:58:53
categories: other
tags: other
---
![](/images/s28.jpg)

# use vi mode in sublime

##  config your user-setting
`Preferences->Settings-User`

```
{
	 # "ignored_packages": ["Vintage"],
	  "ignored_packages": [] #to use Vintage to support vi mode
}

```


## keymap instead Esc

```
{ "keys": ["j","j"],  "command": "exit_insert_mode",
		"context":
		[
			{"key": "setting.command_mode","operand":false},
			{"key": "setting.is_widget","operand":false}
		]
},

```




## my keymap config:

```
[
	{ "keys": ["ctrl+n"], "command": "advanced_new_file_new"},
	{ "keys": ["ctrl+enter"], "command": "goto_python_definition"},
	{ "keys": ["alt+1"], "command": "toggle_side_bar" },
	{ "keys": ["ctrl+f12"], "command": "side_bar_files_open_with",
	             "args": {
	                "paths": [],
	                "application": "/usr/bin/X11/google-chrome",
	                "extensions":".*" //any file with extension
	            } 
	},
	{ "keys": ["ctrl+alt+l"],  "command": "htmlprettify"},
	{ "keys": ["j","j"],  "command": "exit_insert_mode",
		"context":
		[
			{"key": "setting.command_mode","operand":false},
			{"key": "setting.is_widget","operand":false}
		]
	},


]
```