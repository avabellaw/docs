# Firefox hardening

## Betterfox

[Download user.js](https://github.com/yokoffing/Betterfox/blob/main/user.js) 

Make a backup of your current profile and then add user.js to the root of the profile. Can access from 'about:profiles'.

**Betterfox fixes unable to edit Google Docs issue itself**

## Fix camera/audio not accessable

in about:config, set:

media.webrtc.camera.allow-pipewire	false

[Alternative methods and more information](https://discussion.fedoraproject.org/t/google-meet-firefox-cannot-access-webcam-and-microphone/147212)
