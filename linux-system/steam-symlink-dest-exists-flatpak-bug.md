https://ideatrash.net/2024/07/howto-update-xdg-user-dirs-to-avoid-symlink-issues-with-flatpak.html

Essentially, just edit config and update to real dirs if you symlink ~/Music -> elsewhere

edit $HOME/.config/user-dirs.dirs and change the appropriate lines:

XDG_MUSIC_DIR="/some/custom/path"
XDG_PICTURES_DIR="/some/custom/path"