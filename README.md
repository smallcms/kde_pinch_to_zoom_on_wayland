# KDE pinch to zoom in Wayland
### Working example, how to enable pinch-to-zoom in Fedora, OpenSUSE, etc RPM-based distro.

# Information
This example based on two packages: well known in Xorg [touchegg](https://github.com/JoseExposito/touchegg) and started in 2022 [dotool](https://git.sr.ht/~geb/dotool). The **dotool** help us with pinch move in Wayland.

# Installation
### Add current user to input group
```
sudo usermod -a -G input $USER
```
and then it's foolproof to reboot to make the group and rule effective
### Install touchegg
Fedora:
```
dnf install touchegg
```
OpenSUSE:
```
zypper install touchegg
```
Mageia
```
urpmi touchegg
```
### Install dotool
See instructions here: [dotool from Open Build Service](https://software.opensuse.org//download.html?project=home%3Asmallcms&package=dotool)

### Post install (run as user)
Enable and start touchegg service
```
sudo systemctl --now enable touchegg.service
```
On Fedora, Mageia post install command udevadm run automatically
```
sudo udevadm control --reload && sudo udevadm trigger
```
Enable and start dotoold service for your current user
```
systemctl --user --now enable dotoold.service
```
# Configuration
### Configure touchegg
Copy touchegg.conf to current user's config directory
```
cp /usr/share/touchegg/touchegg.conf ~/.config/touchegg/touchegg.conf
```

Open local ~/.config/touchegg/touchegg.conf. Find xml section ```<application name="All">``` and add:
```
    <gesture type="PINCH" fingers="2" direction="IN">
      <action type="RUN_COMMAND">
        <repeat>true</repeat>
        <command>echo key super+minus | dotoolc</command>
        <decreaseCommand>echo key super+equal | dotoolc</decreaseCommand>
        <on>begin</on>
      </action>
    </gesture>

    <gesture type="PINCH" fingers="2" direction="OUT">
      <action type="RUN_COMMAND">
        <repeat>true</repeat>
        <command>echo key super+equal | dotoolc</command>
        <decreaseCommand>echo key super+minus | dotoolc</decreaseCommand>
        <on>begin</on>
      </action>
    </gesture>
```
Note: if your hot keys not Meta+=/Meta+-, use your own from result of ```dotool --list-keys```
### Configure dotool
No configuration needed (John Gebbie, you rocks!).
# F.A.Q.
**Q: Why use dotoold/dotoolc combination? Command ```echo key super+minus | dotool``` works same...**

**A:** No, not same. dotoold with dotoolc calls works better and faster.
