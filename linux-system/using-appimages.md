# Using appimages 

1. Make executable 
2. Run

## Add to search menu

1. Save in a dir such as .appimages in Home
2. Create an [appName].desktop file in ~/.local/share/applications
3. Paste the following and edit the values after '[Desktop Entry]'

```
[Desktop Entry]
Version=0.13.23
Type=Application
Name=appName
Comment=Application Description
TryExec=Path/to/AppImage
Exec=Path/to/AppImage
Icon=Path/to/AppImage.icon
Actions=Editor
```