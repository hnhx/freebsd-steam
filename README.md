# Steam on FreeBSD

Load in the `linux` and `linux64` kernel modules with
```
sudo kldload linux linux64
```
to make this process automatic at boot:
```
sudo sysrc kld_list+="linux linux64"
```

The linux-steam-utils package is usually outdated therefore we will use the upstream repo:

```
git clone https://github.com/shkhln/linuxulator-steam-utils.git
cd linuxulator-steam-utils
sudo make dependencies
make
sudo make install
```

Add this to your `/etc/fstab`, its required for linux compatiblity:
```
linprocfs       /compat/linux/proc     linprocfs        rw              0 0
linsysfs        /compat/linux/sys      linsysfs         rw              0 0
tmpfs           /compat/linux/dev/shm  tmpfs            rw,mode=1777    0 0
fdesc           /dev/fd                fdescfs          rw              0 0
proc            /proc                  procfs           rw              0 0
```

Reboot.

Install and start steam.
```
/opt/steam-utils/bin/steam-install --allow-stealing-my-passwords,-browser-history-and-ssh-keys
/opt/steam-utils/bin/steam --allow-stealing-my-passwords,-browser-history-and-ssh-keys
```

And it should work fine!

# Wine/Proton support

First install Proton 6 by starting a Windows game.

```
sudo pkg install wine-proton libc6-shim python3
mkdir -p $HOME/.i386-wine-pkg/usr/share/keys
ln -s /usr/share/keys/pkg $HOME/.i386-wine-pkg/usr/share/keys
/usr/local/wine-proton/bin/pkg32.sh install wine-proton mesa-dri
/opt/steam-utils/bin/lsu-register-proton
```
Restart Steam and choose `emulators/wine-proton` from the available proton versions.
