+++
date = '2025-09-21T14:53:06+01:00'
title = 'How I Manage All My Configuration Files'
+++
At one point or another, every developer will have to set up their workspace on a completely new machine. The reason could be anything, but what is certain is that this day will come sooner or later. And when it comes, be sure to have prepared for it so it doesn’t take you days, but hours at most.

How do we prepare for it? Well, the biggest workload when setting up a workspace is configuration. It takes time to set up partitions or install mandatory applications. But the biggest gotcha is configuring your applications to fit your needs. Be it a simple VS Code config file or a complete Neovim IDE, it does not get better the longer you work in this field.

But there is a simple solution I found that has no dependencies and is straightforward to put in place. And it also works on both Linux and macOS! So, let’s get to work!

## Requirements

- Basic knowledge of the CLI.

# The problem we are trying to solve.

The goal is to push all important configuration files to a Git repository. This is not demanding in itself, but there is one important caveat to consider.

**The configuration files are not in one place, but are actually spread across the whole system.** This means we can’t push one folder to Git because we need the dotfiles at different locations in the system. And if we were to push one folder to Git, we would need to replace the configuration file each time we make a change. No bueno.

The solution is actually pretty straightforward. We first create one folder where we put every single one of our configuration files. Then we create symlinks for each file to its desired location. The symlink then makes a copy to that location without actually creating a new file. Now we have a central location where we manage all our configuration files in one place. This allows us to create a Git repository in that folder.

# How symlinks are the way to go.

Before we start implementing, let’s take a moment to define symlinks. The word symlink is a short form of symbolic link. Symbolic links are file-system objects that point toward another file or folder. These links serve as shortcuts. They have special features that let users reach files in different folder locations. They give operating systems directions to find the “target” file.

These types of links began to appear in operating systems in the late '70s, such as RDOS. In modern computing, most Unix-like operating systems support symbolic links. These systems follow the POSIX standard. They are often used in cloud storage services like OneDrive, Google Drive, or Dropbox.

Symbolic links are further divided into two types: hard links and soft links. Hard links are files that point to the same data in storage as the original file. Thus, whenever you change the hard link, you also change the original. This not only includes the content but also permissions, ownership, and timestamps. And when you remove the original file, the data still exists under the secondary hard link. The data is only removed from your drive when there are no links to it.

Soft links are what we focus on. They are often the links people mention when talking about symbolic links. A soft link is different from a hard link. It isn’t a real file. Instead, it’s a special file that points to the original file. So every time you open and edit a soft link, what you are actually doing is editing the original file. This also means that if you delete or move the original file of the soft link. The pointer of the soft link is now broken, which makes the data unusable. Be sure to keep this in mind.

# Creating our first symlink.

Now that we know what symlinks are, the solution seems simple. All we need to do is move our important config files into one monorepo. Then, we can set up symlinks to their right locations. Let’s see this in action.

Suppose we have a .bashrc with our custom configuration that we want to push to Git. The first thing we need to do is create a folder for our repo. I’ll create mine at ~/Desktop/dotfiles. I will also create a folder for the .bashrc to maintain structure. This will come in handy when we have several configuration files; trust me.

```
mkdir -p ~/Desktop/dotfiles/bash
cd ~/Desktop/dotfiles/bash
```

We need to move our .bashrc file to the new folder. Then, we'll create a symlink to its original location. You can create symlinks in different ways. But the easiest method is to go to the desired location and type the command: `ln -s <FILE_LOCATION>`.

```
mv ~/.bashrc ./
cd ~
ln -s ~/Desktop/dotfiles/bash/.bashrc
```

This will create the symlink in the current directory with the same name as the original file. It might seem a bit hard at first, but after you’ve done it a couple of times, it becomes easy. In fact, you could say it gets… *soft*. (Because, you know, *soft links*.)

Once you set this up for your other config files, your dotfiles repo will become your personal toolkit for any new machine. The best part? The next time you switch computers, all you’ll need to do is clone your repository and re-establish the symlinks. In just a few commands, your familiar setup will be back, making time for the important things.

Managing dotfiles this way is simple, portable, and future-proof. You can set up a new laptop, start a cloud server, or try things on a VM. Your environment is always ready for you. Think of it as your developer “save file” — one you can load anywhere, anytime.
