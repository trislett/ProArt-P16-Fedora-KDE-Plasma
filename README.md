# ProArt-P16-Fedora-KDE-Plasma
Issues/fixes getting linux to work on the Asus ProArt P16

A lot of this is from my history. I will try to make is as reproducible as possible.

1. Give up on install arch, and install install Fedora. I couldn't get the graphics to display properly no matter what kernel I used. I was able to get past the black screen, but that's about it.
  

2. Installing asusctl and supergfxctl

```
sudo dnf copr enable lukenukem/asus-linux
sudo dnf install asusctl supergfxctl
sudo systemctl enable supergfxd.service
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo systemctl start asusd.service 
sudo systemctl enable asusd.service
```

3. Enable KMS for the nvidia drivers (not sure if necessary).

```
sudo grubby --update-kernel=ALL --args="nvidia-drm.modeset=1"
```

4. Set-up a larger swap file (optional)

```
sudo rm -rf /swapfile 
sudo btrfs filesystem mkswapfile --size 24G /swapfile
sudo swapon /swapfile
```

4. Fix screen flicker. The screen will flicker black occassionally which is quite annoying. This was difficult to figure out, but it's due to the amdgpu not nvidia.
The fix is to disable panel self refresh.
```
sudo grubby --update-kernel=ALL --args="amdgpu.dcdebugmask=0x10"
```

5. Remove bloated, unnecessay KDE tools (optional). These tools took a surprising amount of RAM.

```
sudo dnf remove korganizer kmail akregator kaddressbook konversation kf5-akonadi-server mariadb-common
```

6. (Partial fix) HDR error on wake. If are using HDR (recommendend), on wake the colors will be become over-saturated. As a band-aid fix, the toggle-hdr script turn HDR off and on fixing the color issue. If someone has less hacky solution, please share.

```
toggle-hdr
```
