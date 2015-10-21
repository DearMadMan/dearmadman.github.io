title: sublime text3 vintage
date: 2015-10-21 16:18:57
categories: [other]
tags: other
---

![](/images/s31.jpg)

# sublime text3 vintage 

## Enabling Vintage

```
"ignored_packages":[]
```

## Vintage Start in command mode

```
"vintage_start_in_command_mode":true
```


## bind keys to exit command mode

```
{
	"keys":["j","j"],"command" : "exit_insert_mode",
	"context":[
		{"keys":"setting.command_mode","operand":false},
		{"keys":"setting.is_widget","operand":false}
	]
}

```

## Holding down a key won't repeat it In mac

```
defaults write com.sublimetext.2 ApplePressAndHoldEnabled -bool false
```