# Tmux
[Tmux](https://github.com/tmux/tmux/wiki) is a terminal thingy which let's you keep your sessions, manage windows and whatsoever :)  
I'm learning from [Youtube tutorial by *Dreams of Code*](https://www.youtube.com/watch?v=DzNmUNvnB04)  
I'm not going into configuration and stuff because I already have this config done in [my dotfiles](https://github.com/cyprich/dotfiles.git) 

# Install
You will need to install `tmux` package with your package manager  
Configuration can be downloaded from [my dotfiles](https://github.com/cyprich/dotfiles.git) 
Run it simply with `tmux` command  

# Basic terms
Tmux consists of 3 main objects:
- Sessions
	- The main, topmost layer of Tmux
	- You can have any number of Sessions open up, but typically only attach to one
	- It contains one or more Windows, one of which is active
- Windows
	- Windows are inside Sessions
	- Similar to tabs in web browser
	- Only one can be active *(marked with `*`)*
	- Consists of one or more Panes
	- Info about Windows is show at the bottom of the screen
- Panes

You have Session, which can be split into Windows, which can be split into Panes  

# Commands/shortcuts/keybinds
Before entering any command, you have to press the `<prefix>` key, which is set to `ctrl + b` by default  

| Shortcut (Window) | Action                                 |
| ----------------- | -------------------------------------- |
| `c`               | Create new Window and set is as active |
| `<window_number>` | Set `<window_number>` as active Window |
| `n`               | Next Window                            |
| `p`               | Previous Window                        |
| `&`               | Kill window                            |
|                   |                                        |
|                   |                                        |

| Shortcut (Pane) | Action                                                                     |
| --------------- | -------------------------------------------------------------------------- |
| `%`             | Split to Panes vertically                                                  |
| `"`             | Split to Panes horizontally                                                |
| `←` `→` `↑` `↓` | Change active Pane                                                         |
| `{` `}`         | Move Pane                                                                  |
| `q`             | Toggle Pane number (Enter the number while toggled to switch to that Pane) |
| `z`             | Toggle zoom Pane to fullscreen                                             |
| `!`             | Make current Pane into window                                              |
| `x`             | Close pane                                                                 |


> Note: As said before, you have to hit `<prefix>` before any of these commands... I just didn't wrote it here because it'll be too long

# Sessions
Tmux sessions can be created by using the `tmux` command whilst not attached to another Tmux session  
You can make names session with this command `tmux new -s name_of_the_session`  

List Tmux session   
```bash
tmux ls  # ouside session
<prefix> s  # inside session
<prefix> w  # inside session + preview
```

Attach to session  
```bash
tmux attach  # most recent session
tmux attach -t name_of_the_session  # specific session
```

Rename session - inside session  
```shell
<prefix> b : 
rename-session new_session_name
```

Rename session - outside session  
```shell
tmux rename-session -t old_session_name new_session_name
```

# Better navigation
