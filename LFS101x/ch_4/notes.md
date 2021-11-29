# Chapter 4: Graphical Interface

**Note**: This chapter is pretty useless for Linux power users. For more information on the graphical user interface, see the [Arch Linux wiki](https://wiki.archlinux.org/title/General_recommendations#Graphical_user_interface) on the topic.

## Tasks
- [x] None

## X Window System

Generally, in a Linux desktop system, the X Window System is loaded as one of the final steps in the boot process. It is often just called X.

A service called the **Display Manager** keeps track of the displays being provided and loads the X server (so-called, because it provides graphical services to applications, sometimes called X clients). The display manager also handles graphical logins and starts the appropriate desktop environment after a user logs in.

X is rather old software; it dates back to the mid 1980s and, as such, has certain deficiencies on modern systems (for example, with security), as it has been stretched rather far from its original purposes. A newer system, known as [Wayland](https://wayland.freedesktop.org), is gradually superseding it and is the default display system for Fedora, RHEL 8, and other recent distributions.  For the most part, it looks just like X to the user, although under the hood it is quite different.

![Display Manager](./img/img_0.png)

## More About X

A desktop environment consists of a session manager, which starts and maintains the components of the graphical session, and the window manager, which controls the placement and movement of windows, window title-bars, and controls.

Although these can be mixed, generally a set of utilities, session manager, and window manager are used together as a unit, and together provide a seamless desktop environment.

If the display manager is not started by default in the default runlevel, you can start the graphical desktop different way, after logging on to a text-mode console, by running `startx` from the command line. Or, you can start the display manager (**gdm**, **lightdm**, **kdm**, **xdm**, etc.) manually from the command line. This differs from running `startx` as the display managers will project a sign in screen. We discuss them next.

![Desktop Environment](./img/img_1.png)

## GNOME Desktop Environment

GNOME is a popular desktop environment with an easy-to-use graphical user interface. It is bundled as the default desktop environment for most Linux distributions, including Red Hat Enterprise Linux (RHEL), Fedora, CentOS, SUSE Linux Enterprise, Ubuntu and Debian.

## Suspending

All modern computers support **Suspend** (or **Sleep**) **Mode** when you want to stop using your computer for a while. Suspend Mode saves the current system state and allows you to resume your session more quickly while remaining on, but uses very little power in the sleeping state. It works by keeping your system's applications, desktop, and so on, in system RAM, but turning off all of the other hardware.
