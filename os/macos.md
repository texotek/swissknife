# Mac OSX

## 1. OS & Architecture

**Kernel**: [XNU](https://github.com/apple/darwin-xnu)

The Mach kernel is the basis of the Kernel architecture.
It handles memory, processes, drivers and other low-level tasks.

**OS Base**: [Darwin](https://github.com/apple/darwin-xnu), a FreeBSD Deriviative (OSS by Apple)

Darwin is the base of macOS. Darwin has been realeased for Open
Source use. Darwin, with other components like *Aqua*,*Finder*
and other *components*, make up the macOS that we know.

## 2. Core components

GUI: [Aqua](https://en.wikipedia.org/wiki/Aqua_(user_interface)#References)
is the basis of the Graphical User Interface and visual Theme.

File Manager: [Finder](https://support.apple.com/en-us/HT201732)
provides the Desktop experience and File Management in the OS. Aqua 
is responsible for launching the applications.

Application SandBox: MacOS and any apps within utilize the concept
of sandboxing the application's outside of the resources necessary for it to run.

[Cocoa](https://developer.apple.com/library/archive/documentation/macOSX/Conceptual/OSX_Technology_Overview/CocoaApplicationLayer/CocoaApplicationLayer.html)
is the application management layer and API used in macOS. It is
responsible for the behavior and built-in apps in macOS.

## 3. Tweaks

### 3.1 Show All Files in Finder

When beeing in Finder, press the **CMD** + **Shift** + **.** Key Combinatin.

or write it in the defaults:
```bash
defaults write com.apple.Finder AppleShowAllFiles true
killall Finder
```

### 3.2 Prevent Workspace switching after quitting app

```bash
defaults write com.apple.dock workspaces-auto-swoosh -bool NO
killall Dock
```
