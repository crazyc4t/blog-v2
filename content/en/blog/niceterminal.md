{
  "title": "How to customize your terminal!",
  "date": "2021-11-14T19:55:31-05:00",
  "draft": false,
  "author": "Said Neder",
  "tags": ["Terminal"],
  "categories": "Linux",
  "toc": true,
  "image": "images/terminal.png"
}

## Intro

This is a tutorial-type of blogpost, sorry for being away so long, but there's a lot of blogposts coming to your way, so expect some new ones!

What we want to achieve in this tutorial:
- To have a fast terminal emulator
- To have a better shell with auto-completition
- To have a cool-looking with color scheme terminal

So let's start with that!

## Alacritty, the fastest openGL terminal emulator

[Alacritty](https://github.com/alacritty/alacritty) is the fastest terminal emulator out there, and the best of all is that is cross-platform! so you can install it on windows too! Although this guide is more toward linux users but if you are here from windows you can follow along too!

## Fish, the friendly interactive shell from the 90's

Fish is a smart and user-friendly shell for every unix-like OS, it offers:
- Autosuggestions
- Sane Scripting
- Tab completitions and syntax highlighting

And a lot more! Overall, is the way to go, time to go [fishing](https://fishshell.com)

## Neovim, Hyperextensible Vim-based text editor
[Neovim](https://neovim.io/) being a [vim](https://www.vim.org/)-based editor is not for everyone, since navigating vim is not the easiest task of all, but neovim what it does is to take vim (VI-iMproved, since vim is a fork from VI) and eliminates all the deprecated and legacy packages (30% less code base than vim), then improve it even more with plugins, extensions, and more!

Is a beutiful text-editor, one of my favourites!

## Nerd Fonts, the best fonts to code with!
[Nerd fonts](https://www.nerdfonts.com/#home) are fonts designed for coding in mind, and it has icons as text that is the awesome part, since we are going to use those icons for customizing our terminal

## Bat, the cat with wings!

`bat` is `cat` but with super powers, with syntax highlighting and supports git integration, and it displays it's text in a well readable format, once you go bat you never go back!

## Install Alacritty, Fish, Bat and Neovim

**Ubuntu/Debian:**

`sudo add-apt-repository ppa:aslatter/ppa && sudo add-apt-repository ppa:fish-shell/release-3`

That is to add the *personal package archives*, that is a personal repository where you can download software from the owner of the software, being the latest version and overall better, in this case from alacritty and fish devs.

`sudo apt update && sudo apt install alacritty bat fish neovim -y`

This will refresh the repo list and install it!

**Fedora/RHEL:**
`sudo dnf install alacritty bat fish neovim` Easy isn't it?

**Arch (btw):**
`sudo pacman -S alacritty bat fish neovim` You already know.

**Uninstalling Alacritty, Fish, Bat and Neovim**

This is the case if you don't like it

**Ubuntu/Debian:**

First, remove the software:
`sudo apt remove --auto-remove alacritty fish bat neovim`

Then, the *PPA's*
`sudo add-apt-repository --remove ppa:aslatter/ppa && sudo add-apt-repository --remove ppa:fish-shell/release-3`

**Fedora/RHEL:**

`sudo dnf remove fish alacritty bat neovim`

**Arch (btw):**

`sudo pacman -R alacritty fish bat neovim`

## Installing nerd fonts!
You can go to the official nerd fonts [github](https://github.com/ryanoasis/nerd-fonts) and follow their installation guides or use a **bash script** I created for installing nerd fonts the easiest way!

**Download the bash script**
```bash
wget https://github.com/DarthNeder/nerdy/releases/download/v1.0.0/nerdy && bash nerdy -h
```

This will download the script and run the help command so you can see the usage of the script

**Install some fonts!**
Is just as easy to 
```bash
bash nerdy RobotoMono
```

This means you are installing RobotoMono nerd font on your system!
If you are not sure which one to install you can go to [programming fonts](https://www.programmingfonts.org/) and decide which one to install!

After installing nerd fonts, neovim, fish and alacritty, time to configurate them!

## Hacking alacritty

First in your new terminal we are going to create the `alacritty.yml` file, this is the configuration file,
and is going to be store in `~/.config/alacritty/`.

So now this is what we are going to do:
```bash
$ cd
$ mkdir .config/alacritty
$ nvim .config/alacritty/alacritty.yml
```

(The first `cd` was for changing directories to your home folder)

Now, before we get into the code, just for reference you can see this alacritty [config](https://github.com/tmcdonell/config-alacritty) to take for example, and the [arch wiki](https://wiki.archlinux.org/title/Alacritty) since it has a nice page explaining about it.

**Running fish as our default shell**
```yaml
shell:
	program: /bin/fish
```

This means everytime alacritty is going to open is running the fish shell.

Remember the [YAML](https://www.redhat.com/en/topics/automation/what-is-yaml) Is the format used to configure alacritty, in key-value pairs.

**Running nerd fonts as our default font**

First of all check the family name of your font by running this command:
```bash
fc-list | grep Hack
```

Being **Hack** replaced with your font's name, once you know that time to create the font table like this:

```yaml
font:  
  normal:
		family: RobotoMono Nerd Font
    style: Regular

  bold:
    family: RobotoMono Nerd Font
    style: Bold

  italic:
    family: RobotoMono Nerd Font
    style: Italic

  bold_italic:
    family: RobotoMono Nerd Font
    style: Bold Italic

  size: 9.0
```
Specifying the font family and the size of it, if you want, (I know you do) we can specify the padding of the prompt and the background opacity of it
```yaml
background_opacity: 0.75
window:
  padding:
    x: 4
    y: 4
```

And we are done with alacritty and fish!

Now we are going to install [starship](https://starship.rs/) to beutify our prompt and plus is written on rust!, then there's [exa](https://the.exa.website/) that is the new replacement for the `ls` command.

## Installing starship and exa

**For starship:**

Just run the installer:
```bash
bash sh -c "$(curl -fsSL https://starship.rs/install.sh)"
## if you are in fish, if not just run without `bash` at the beginning
```

and add this to the config.fish, that is located in `~/.config/fish/config.fish`
```fish
# ~/.config/fish/config.fish

starship init fish | source
```

**Side note:**

If you don't like the "welcome to the fish shell!" greeting message, you can delete it by so:

`set -g fish_greeting` 

writing this in `config.fish`.

More [docs](https://fishshell.com/docs/current/faq.html)...

**For exa:**

**Ubuntu/Debian:** `sudo apt install exa`

**RHEL/Fedora:** `sudo dnf install exa`

**Arch Linux (btw):** `sudo pacman -S exa`

And that's it! now you have starship and exa in your terminal!

**Now replace `cat` and `ls` with aliases:**

In `config.fish`:

```fish
alias ls="exa -la --icons"
alias cat="bat"
```

Now last but not least final step...

## Theming Alacritty

First of all let's install [node.js](https://nodejs.org/en/) that makes running javascript on the backend possible!

**Ubuntu/Debian:** `sudo apt install nodejs`

**RHEL/Fedora:** `sudo dnf install nodejs`

**Arch (Btw):** `sudo pacman -s nodejs`

Now let's install [alacritty themes](https://github.com/rajasegar/alacritty-themes)!
```bash
npm i -g alacritty-themes
```

If you get npm errors, try to update npm:
`npm update`

**Now the moment of truth!**

Just run the script!
`alacritty-themes`

and select the theme you like and enjoy!

![Custom terminal](/images/terminal.png)
