# 001 - Installing Software and Preparing Development Environment

---
## Overview
We are going to be setting up our development machine. For the purpose of this tutorial I will be using Linux (Ubuntu 16.04) 64-bit Desktop OS. We are going to setup NodeJS and NPM using a specific process that allows us to grab the most current version. Furthermore, I will be setting up an IDE (which I will be using throughout the tutorials) and a text-editor with some plugins meant for NodeJS development within it.

Note: In the next tutorial I will cover some very simple 'Hello World!' type scripts and launching them with debugging capabilities using three different options: (1) command-line text editor (nano) with attached default Node Debugger tools; (2) Atom with plugins for debugging; (3) Webstorm IDE with built-in debugger. Afterwards I will only be using Webstorm in my tutorial videos, but hopefully the initial coverage of Atom will suffice to allow viewers to adapt and carry out the same basic tasks within Atom. For now we are just going to install these things and configure them.


#### About Webstorm IDE

[Link: Webstorm Homepage and Download Site](https://www.jetbrains.com/webstorm/)

Webstorm is definitely (in my opinion and experience) the best IDE available for NodeJS development as it incorporates an editor, project manager, toolbars, tools/utilites (npm, testing frameworks, etc), syntax highlighting, shortcuts, command-line terminals, console, and - perhaps most importantly - a solid set of debugging tools. The downside to all of this is that it is a subscription-based license model and it is not very cheap. They do offer a 30 day free-trial. Years ago the expiration of this trial period really didn't prevent you from using the product, rather you lost access to upgrades and, if I remember correctly, you had to save your work, close it, and re-open it every 30 minutes of continued use. I am not sure that remains the case today; I believe it has gotten much stricter. I have had a paid account for a few years now so I cannot say for certain. I have also read about people proposing that the trial is tied to a specific file on your system which can be removed or bypassed by creating a new local user account on your OS and re-installing the IDE to it. This very well may violate their Terms of Service if it even works - I leave it up to you to investigate and research on these things as I'm not promoting or condoning piracy or TOS violations. If you are a student - you may be eligible to receive an extended license through your school. In that case I would urge you to send an email to, or otherwise contact, Jetbrains and see what can be done for you.


#### About Atom Editor w/ plugins

[Link: Atom Homepage and Download Site](https://atom.io/)

Atom is a free text-editor. In some ways it is similar to Sublime Text, but a whole lot better in my opinion. It allows heavy customization via plugins that can transform it from a basic text-editor into more of a complete IDE for any number of languages. Since Webstorm only offers a free-trial for 30 days (and costs a pretty large amount afterwards for someone who is only learning), I figured I would at least propose this as an alternative option. It is by no means as comprehensive, user-friendly, or "clean" compared to Webstorm, but it can get the job done.

A quick list of my Atom plugins:

**packages**
- atom-ternjs
- autoclose-html
- bottom-dock
- editorconfig
- highlight-selected
- markdown-writer
- node-debugger
- pigments
- project-manager
- terminal-plus
- todo-manager
- tool-bar
- tool-bar-markdown-writer

**themes**
- seti-ui
- atom-monokai


## About Operating Systems

As I mentioned - I will be using Linux (Ubuntu 16.04). NodeJs was originally developed for Linux, but it was ported to Windows and Mac OS X soon afterwards. Personally I find development on Windows to be a bit of a nightmare. (Windows 10 has introduced virtual desktops and is finally integrating the BASH shell command-line which are very positive steps in the right direction). Mac OS X is similar to Linux (it is partially built on Linux), so it is a bit more friendly.

All the same it comes down to personal preference and what you have access to. I may decide at a later date (if there is demand for it) to create an additional video or two covering the contents of this setup for Windows and Mac OS X, but for the time being it will strictly be Ubuntu/Linux.

I mentioned this in the previous tutorial page, but to reiterate it: in previous years I put together some tutorials on setting up Ubuntu/Linux as a Dual-boot option along-side Windows allowing you to choose which OS to load when you boot your computer. The only requirement is some available hard drive space and a USB thumb drive or CD/DVD ROM drive/burner. Alternatively, I created additional videos in that tutorial series regarding installing Ubuntu as a Virtual Machine Guest inside of Virtualbox hypervisor software running on top of Windows 7 host. The only requirement is some available hard drive space (~20GB+) and a decent quantity of RAM (if you have 8GB+ you will have no problem; less than that is possible but performance can be poor).

Those videos/tutorials can be found [here on my YouTube channel](https://www.youtube.com/watch?v=rDkfEoAEKp4&list=PLKYLJoiyP2a5GEmUH4W58nw8-9-UFv65R) (you probably want videos 3,4, and/or 5)


## Why Not Use NodeJS Package via Default Package Manager (e.g. *apt* repo)

- Traditional package manager sources are often outdated. For example in Ubuntu 14.04 installing with *apt* will default to **nodejs v0.10.37**, and with Ubuntu 16.04 it will install **nodejs v4.2.6** (I believe), despite the fact that the current version is **nodejs v6.7.0** as of this writing. Furthermore, Ubuntu's default official package and binary is named **nodejs** rather than **node** to avoid conflict with an older package with that same name. This can cause issues if executing some 3rd party code which attempts to invoke NodeJS using an expectation that the binary is invoked with the command **node** (which is fairly standard).

- One way around this would be to change to your global user binary directory and create a symbolic-link to the *nodejs* binary named *node*:
  - `whereis nodejs`
    - output => /usr/bin/node /usr/include/node /usr/share/man/man1/node.1.gz
  - `cd /usr/bin/`
    - change to location of the binary
  - `sudo ln -s nodejs node`
    - creates a symbolic-link which says "node" points to "nodejs", allowing users to globally invoke **nodejs** by calling to just **node** command
  - Doing this works - but it still doesn't solve the outdated version issue, until/unless Ubuntu's own apt sources are updated with a newer version (which is out of your control).

- Another alternative that was pushed heavily was to use something like **nvm** (Node Version Manager). This is a flawed approach as it will install and manage various versions of NodeJS and NPM all within your user's *home* directory. This means that other users on your system will not have access to it, including *root* user. Very quickly this can lead to a series of attempts to fix things by altering permissions and creating chains of symbolic-links that can become unmanageable and break when you try to switch to a different version of NodeJS with NVM (which was really the entire purpose and benefit of it to begin with). One could argue that Node and NPM should never require **root** (sudo) access, however there are definitely situations where that logic does not hold up.

- Finally, the way I will be demonstrating and using: Use [deb.nodesource.com](deb.nodesource.com) to grab Linux Distro-dependent shell-script; The shell script will run on your system and add some directives/source-references to your */etc/apt/sources.list.d/* \* directory. In turn this will allow you to use *apt* to install, remove, update, and upgrade nodejs to the most up-to-date version available directly from nodesource site.

---
## Installing NodeJS

**prerequisite**

If you know you do not have NodeJS installed already on your system then skip directly down to the **process** section, otherwise read on.

If you already have nodejs installed on your system you may need to remove it first. Depending how it was initially installed you can either use `sudo apt-get remove node` or `sudo apt-get remove nodejs` , or you may need to manually wipe it out. Usually you can check `ls -slah /usr/bin/ | grep node*` to see if it is there. Then simply `sudo rm /usr/bin/node*`. Same can be done for *npm*.

You can check if you have either of them by simply running `node -v` and `npm -v` from command line. You should also check `nodejs -v`. If any of them exist, you can try `whereis node`, `whereis nodejs`, and/or `whereis npm`. Alternatively, sometimes replacing `whereis` with `which` can be useful for locating binaries in your PATH.

** process **

1. make sure your system is up-to-date: `sudo apt-get update`
2. **(optional)**: you may opt to upgrade all software packages and even the core/kernel modules and dependencies using `sudo apt-get upgrade -y` or `sudo apt-get dist-upgrade`)
3. install *wget* package: `sudo apt-get install wget`
  - we'll use this to grab the shell script, for security reasons as I will explain.
4. open terminal (Ctrl + Alt + T usually works as a shortcut for this, at least in Unity DE)
5. open browser and navigate to [this stack-overflow post](http://stackoverflow.com/questions/34974535/install-latest-nodejs-version-in-ubuntu-14-04)
  - Note the top answer by user: war1oc . In fact I have made a comment about my issue with on that post as user: Brandon K . We are going to effectively mitigate that security issue I mention in the commend.
  - the recommendation for Node.js v6 in that answer is:
    - `curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - `
    - `sudo apt-get install -y nodejs`
  - The problem with this - the command says use *curl* and grab the page/file at the URL and then pipe it directly into Bash, executing whatever code happens to be in that script, all with sudo (*root*) privileges. If that domain or site were ever compromised and someone replaced the shell script there with one that did something malicious, you would be willingly executing it on your machine with full root access granted ... not good. So I say we at least take some time to check it out as follows.
6. from command line run `wget https://deb.nodesource.com/setup_6.x -O add_nodejs_6x.sh`
  - a few important parts to recognize:
    - the url contains `deb.` - this says grab the script for debian (which Ubuntu is derived from); Should you be using RedHat/Centos/Fedora you can replace this part of the url with `rpm.` instead.
    - the url contains `_6.x` for version. If you wanted version 5 swap this with `_5.x` instead, etc.
    - the `-O add_nodejs_6x.sh` says to output the results of *wget* retrieval to a named file. The name I chose here is arbitrary, but the `.sh` extension indicates that it is a shell script so it is recommended.
  - You could just go directly to [nodesource github page](https://github.com/nodesource/distributions) in browser and grab the files that way or review them there as well
7. When complete, you can review the file to see exactly what it does. `nano add_nodejs_6x.sh` and read through it to get a feel for what it does. Essentially it will scan through your system and note what distro and release version you are running within the debian family in order to properly setup your package manager's sources list. For Ubuntu it will generate 3 new files at the system directory path `/etc/apt/sources.list.d/`
  - nodesource.list
  - nodesource.list.distUpgrade
  - nodesource.list.save

When you run updates/upgrades or install commands using *apt* it will check here for references to upstream repositories to retrieve software packages from.

8. make the shell script executable: `chmod a+x add_nodejs_6x.sh`
9. execute the shell script with sudo: `sudo ./add_nodejs_6x.sh`
10. run a quick update sync: `sudo apt-get update`
11. install node: `sudo apt-get install nodejs`
12. close terminal and reopen new one
13. verify nodejs: `node -v` -> should output *v6.7.0* or similar
14. verify npm: `npm -v` -> should output *v3.10.3* or similar
15. just to familiarize yourself with location of binary: `whereis node`
  - should output *node: /usr/bin/node /usr/include/node /usr/share/man/man1/node.1.gz*
  - the first of those absolute paths is to the executable binary invoked with **node** command globally
---

## Installing and Setting Up Development Environment Software
Here I will download, install, and customize/configure IDE and Editors.

### Install & Configure Webstorm IDE
- [Open this link](https://www.jetbrains.com/webstorm/download/#section=linux-version) to the Jetbrains Webstorm site download page
- Download the version for your Operating System of choice (Linux in my case)
- Navigate to your Downloads folder and open the file; It will be a **.tar.gz** file

### Install & Configure Atom Editor with Plugins


---
## Useful and Relevant Links:

- [My Tutorial Video Covering This Page on YouTube]()
- [Setting Ctrl+Alt+T for Terminal Shortcut](http://askubuntu.com/questions/699555/how-to-make-ctrl-alt-t-open-new-terminal-window-when-one-is-already-open)
- [StackOverflow post referencing this NodeJS installation method](http://stackoverflow.com/questions/34974535/install-latest-nodejs-version-in-ubuntu-14-04)
- [NodeSource Github](https://github.com/nodesource/distributions)
- [Webstorm IDE]()
- [Atom Editor]()
- [NPM Homepage]()
- [NodeJS Homepage]()
