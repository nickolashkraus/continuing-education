# Chapter 8: Finding Linux Documentation

## Tasks
- [x] None

## Linux Documentation Sources

Important Linux documentation sources include:
* The **man** pages (short for **man**ual pages)
* **GNU Info**
* The **help** command and **--help** option
* Other documentation sources, e.g. [Gentoo Handbook](https://www.gentoo.org/support/documentation) or [Ubuntu Documentation](https://help.ubuntu.com/community/CommunityHelpWiki)

## The man pages

The man pages are the most often-used source of Linux documentation. They provide in-depth documentation about many programs and utilities, as well as other topics, including configuration files, and programming APIs for system calls, library routines, and the kernel. They are present on all Linux distributions and are always at your fingertips.

## man

A given topic may have multiple pages associated with it and there is a default order determining which one is displayed when no options or section number is specified. To list all pages on the topic, use `-f` option. To list all pages that discuss a specified topic (even if the specified subject is not present in the name), use the `–k` option.

* `man –f` generates the same result as typing whatis.
* `man –k` generates the same result as typing apropos.

## The GNU Info System

The next source of Linux documentation is the GNU Info System.

This is the GNU project's standard documentation format, which it prefers as an alternative to `man`. The Info System is basically free-form, and supports linked subsections.

## Using info from the Command Line

Typing `info` with no arguments in a terminal window displays an index of available topics. You can browse through the topic list using the regular movement keys: arrows, Page Up, and Page Down.

You can view help for a particular topic by typing `info <topic name>`. The  system then searches for the topic in all available info files.

## The `--help` Option

Another important source of Linux documentation is use of the `--help` option.

Most commands have an available short description which can be viewed using the `--help` or the `-h` option along with the command or application.

## The help Command

To view a synopsis of these built-in commands, you can simply type `help`.
