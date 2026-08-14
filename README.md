My i3 Rice

A personal, Linux-first i3wm desktop configuration focused on a clean, themed, keyboard-friendly workflow.

The setup uses Pywal as the color source, so the wallpaper can drive the colors used by the rest of the desktop.

Features

i3wm window manager

Polybar status bar

Picom compositor

Rofi application launcher

Dunst notifications

Pywal wallpaper-based color generation

Kitty terminal

Pavucontrol audio control

Playerctl media controls

Brightnessctl brightness controls

NetworkManager + nm-applet

Blueman Bluetooth manager

Nautilus file manager

GNOME Software

spf / Superfile for terminal file management

Btop system monitor

Git + GNU Stow dotfile management

JetBrains Mono + Nerd Fonts / Symbols Nerd Font

Optional GTK/XDG portal support for standalone i3 sessions

PACKAGES REQUIRED
The package names below are written for Arch Linux / Arch-based systems.
Core desktop
i3
polybar
picom
rofi
dunst
kitty
feh
pywal
git
stow
Audio / media / hardware control
pavucontrol
playerctl
brightnessctl
pipewire
wireplumber
Network / Bluetooth
networkmanager
network-manager-applet
blueman
File management / desktop utilities
nautilus
gnome-software
btop
Fonts
ttf-jetbrains-mono
ttf-jetbrains-mono-nerd
ttf-nerd-fonts-symbols-mono

Optional but useful

xdg-desktop-portal
xdg-desktop-portal-gtk
xfce4-settings (useful for GTK settings in standalone i3/X11 sessions)
i3lock
ffmpeg
superfile
Package availability can vary by Arch-based distribution. Check the package name with pacman -Ss <package> or your distro's package manager if one of these names is unavailable.
Arch installation

Install the main packages:

sudo pacman -Syu
sudo pacman -S \
  i3 polybar picom rofi dunst kitty feh \
  pavucontrol playerctl brightnessctl \
  pipewire wireplumber \
  networkmanager network-manager-applet blueman \
  nautilus gnome-software btop \
  git stow \
  ttf-jetbrains-mono ttf-jetbrains-mono-nerd ttf-nerd-fonts-symbols-mono

Optional extras:

sudo pacman -S \
  xdg-desktop-portal xdg-desktop-portal-gtk \
  xfce4-settings i3lock ffmpeg

Enable the important user services

systemctl --user enable --now pipewire pipewire-pulse wireplumber

Enable NetworkManager:

sudo systemctl enable --now NetworkManager

AUR / paru

This setup can use AUR packages when needed.

If paru is already installed:

paru -S <package-name>

Update system + AUR packages:

paru -Syu

If you are on Arch and do not have an AUR helper yet, install one according to its official documentation rather than blindly copying an old bootstrap script.

Clone the dotfiles

Clone this repository:

git clone https://github.com/ash-astrea-240/Dotfiles-my-rice-.git ~/dotfiles

Enter it:

cd ~/dotfiles

GNU Stow

This repository is intended to be managed with GNU Stow.

The basic idea is:

~/dotfiles/
├── i3/
│   └── .config/i3/...
├── polybar/
│   └── .config/polybar/...
├── picom/
│   └── .config/picom/...
├── rofi/
│   └── .config/rofi/...
├── dunst/
│   └── .config/dunst/...
└── kitty/
    └── .config/kitty/...

From ~/dotfiles, stow each package:

stow i3
stow polybar
stow picom
stow rofi
stow dunst
stow kitty

Or all at once, if every top-level directory is intended to be stowed:

stow */

Important

Do not blindly stow a directory if you already have real config files in the target location. Move or back them up first so Stow can create its symlinks cleanly.

Pywal wallpaper workflow

The main theme is generated from the wallpaper.

Set a wallpaper and generate the palette:

wal -i ~/Pictures/Wallpapers/your-wallpaper.jpg

Set the wallpaper with feh:

feh --bg-fill ~/Pictures/Wallpapers/your-wallpaper.jpg

A useful i3 startup order is:

exec --no-startup-id wal -i ~/Pictures/Wallpapers/your-wallpaper.jpg
exec --no-startup-id feh --bg-fill ~/Pictures/Wallpapers/your-wallpaper.jpg
exec --no-startup-id picom --config ~/.config/picom/picom.conf
exec --no-startup-id dunst
exec --no-startup-id polybar main

Pywal should run before programs that read its generated colors.

For an automatic wallpaper picker, the recommended future workflow is:

Rofi wallpaper picker
        ↓
select image
        ↓
wal generates colors
        ↓
feh sets wallpaper
        ↓
Polybar / Rofi / Dunst / Kitty reload their theme

i3 shortcuts

$mod is normally the Super / Meta key.

Shortcut

Action

Meta + Enter

Open Kitty

Meta + D

Open Rofi application launcher

Meta + F

Toggle fullscreen

Meta + Shift + R

Reload i3 configuration

Meta + Shift + E

Exit i3

Meta + [direction]

Focus window

Meta + Shift + [direction]

Move window

Meta + [number]

Switch workspace

Meta + Shift + [number]

Move window to workspace

Media / hardware controls

The desktop config also supports dedicated media keys where available:

Play / Pause → playerctl play-pause

Next → playerctl next

Previous → playerctl previous

Volume up/down → wpctl

Mute → wpctl

Brightness up/down → brightnessctl

Reload i3 after editing the config:

i3-msg reload

or use:

Meta + Shift + R

Testing individual components

Rofi

rofi -show drun

Polybar

Check the bar name:

grep '^\[bar/' ~/.config/polybar/config.ini

Then start it:

polybar main

Replace main with your actual bar name.

Picom

picom --config ~/.config/picom/picom.conf

Dunst

dunst &
notify-send "Test" "Dunst is working."

Playerctl

playerctl status
playerctl play-pause

Audio

wpctl get-volume @DEFAULT_AUDIO_SINK@

Brightness

brightnessctl get

Bluetooth

blueman-manager

Network tray

nm-applet

Git workflow

This repository is version-controlled intentionally so configuration changes can be tested and reverted safely.

Check what changed

cd ~/dotfiles
git status

See the exact changes

git diff

Save a new version

git add .
git commit -m "Describe what changed"
git push

Example:

git add .
git commit -m "Update Rofi and Dunst theme"
git push

See commit history

git log --oneline

Inspect an older commit

git show <commit-hash>

Restore one file from an older commit

git restore --source <commit-hash> -- path/to/file

Recommended workflow

edit
  ↓
test
  ↓
git status
  ↓
git diff
  ↓
git add .
  ↓
git commit -m "what changed"
  ↓
git push

Make a commit whenever you reach a known-good desktop state.

Notes for standalone i3 sessions

Because i3 is a window manager rather than a complete desktop environment, some desktop services normally supplied by GNOME/KDE need to be started separately.

Common examples:

NetworkManager applet

Bluetooth applet/manager

Dunst

Picom

Polybar

XDG desktop portal

GTK settings daemon

Keep your GNOME installation as a fallback/configuration environment if you use one. It is also useful for diagnosing GTK, portal, or hardware-integration issues.

Superfile

spf / Superfile is a terminal-based file manager.

Launch it with:

spf

Because it is TUI-based, it does not depend on GTK theming in the same way graphical file managers do.

Theme philosophy

The intended theme is:

dark brown / pinkish grey
        +
      pink
        +
      muted green

Pywal provides the wallpaper-derived palette, while important semantic colors can be mapped manually so the desktop keeps a consistent visual hierarchy.

The goal is to keep:

Pink → primary accent / active UI

Green → secondary accent / system information

Dark brown / pink-grey → backgrounds and surfaces

Backup first

Before changing a large part of your configuration:

git status
git add .
git commit -m "Known good configuration"
git push

Then experiment freely.

If something breaks, Git gives you a known-good point to return to.

Current setup

This repository is a personal rice rather than a universal installer. Package names, paths, monitor layouts, keyboard layouts, and application availability can differ between systems.

The safest approach is:

clone → inspect → stow → test → customize

Enjoy the rice.
