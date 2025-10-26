# ProArt-P16-Fedora-KDE-Plasma
Issues/fixes getting linux to work on the Asus ProArt P16

Following the steps below, nearly every feature works out of the box on Fedora KDE Plasma 42. I haven’t done extensive battery‐life testing yet. The primary issues were related to the graphics. However, HDR output functions flawlessly, HDMI output works without any issues, and the hybrid GPU switching is supported. Overall, the ProArt P16 is an excellent Linux device with very few compromises.

Warning: A lot of this is from my bash history. I will try to make is as reproducible as possible.
Of course, run sudo dnf update when necessary.

1. Give up on trying to install arch, and install install Fedora KDE Plasma. I couldn't get the graphics to display properly no matter what kernel I used. I was able to get past the black screen, but that's about it.
  

2. Installing asusctl and supergfxctl

```
sudo dnf copr enable lukenukem/asus-linux
sudo dnf install asusctl supergfxctl
sudo systemctl enable supergfxd.service
sudo systemctl start asusd.service 
sudo systemctl enable asusd.service
```

3. Enable RPM fusion for non-free drivers

```
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm   https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

4. Enable KMS for the nvidia drivers (not sure if necessary).

```
sudo grubby --update-kernel=ALL --args="nvidia-drm.modeset=1"
```

5. Set-up a larger swap file (optional)

```
sudo rm -rf /swapfile 
sudo btrfs filesystem mkswapfile --size 24G /swapfile
sudo swapon /swapfile
```

6. Fix screen flicker. The screen will flicker black occassionally which is quite annoying. This was difficult to figure out, but it's due to the amdgpu not nvidia.
The fix is to disable panel self refresh.
```
sudo grubby --update-kernel=ALL --args="amdgpu.dcdebugmask=0x10"
```

7. Remove bloated, unnecessay KDE tools (optional). These tools took a surprising amount of RAM.

```
sudo dnf remove korganizer kmail akregator kaddressbook konversation kf5-akonadi-server mariadb-common
```

8. (Partial fix) HDR error on wake. If are using HDR (recommendend), on wake the colors will be become over-saturated. As a band-aid fix, the toggle-hdr script turn HDR off and on fixing the color issue. If someone has less hacky solution, please share. 

```
toggle-hdr
```

9. The nested ASUS Dial is not support. I don't really care abou this much. But, if it is a must for you check out:

```
https://github.com/asus-linux-drivers/asus-dialpad-driver
```
