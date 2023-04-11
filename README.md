# Screen tearing Lenovo Thinkpad T14 Gen3 (Intel)

Got a new Thinkpad T14 Gen3 (Intel) and was experiencing screen tearing on both my
internal and external displays when using `i3` window manager as part of the Regolith
DE. This occurred even when using `picom`. Tweaking picom settings didn't get me
anywhere, but I didn't document what I tried. I started documenting after changing focus
from picom.


## Firefox

**No effect.**

In the "Performance" setting, uncheck "Use recommended" and check "Use hardware
acceleration when available".


## TearFree

**Fixes screen tearing in Firefox.**

Create `/usr/share/X11/xorg.conf.d/20-intel-gpu.conf` with:

```
Section "Device"
   Identifier  "Intel Graphics"
   Driver      "intel"
   Option      "TearFree" "true"
EndSection
```

_NOTE: This seems to cause artifacts in Flatpak apps (Slack)._

### TODO

* Try `Option "TripleBuffer" "true"`?
* Try `Option "DRI" "3"`?


## Flatpak

**Fixes Slack artifacts.**

The artifacts are not exactly "screen tearing": regions of the application flash between
blank and normal, and some sections seem to be transposed to a different location.

Install `mesa-git` graphics library with flatpak:

```
flatpak install flathub-beta org.freedesktop.Platform.GL.mesa-git
flatpak remove org.freedesktop.Platform.GL.mesa  # Remove "all of the above"
```
