# ProArt-P16-Fedora-KDE-Plasma
Issues/fixes getting linux to work on the Asus ProArt P16

A lot of this is from my history. I will try to make is as reproducible as possible.

1. Give up on install arch, and install install Fedora. I couldn't get the graphics to display properly no matter what kernel I used. I was able to get past the black screen, but that's about it.
  

2. Installing asusctl and supergfxctl

sudo dnf copr enable lukenukem/asus-linux
sudo dnf install asusctl supergfxctl
sudo systemctl enable supergfxd.service
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo systemctl start asusd.service 
sudo systemctl enable asusd.service
sudo grubby --update-kernel=ALL --args="nvidia-drm.modeset=1"
