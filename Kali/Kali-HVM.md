# Kali HVM

## Kali HVM test

~~~
$ sha256sum kali-linux-2017.1-amd64.iso 
49b1c5769b909220060dc4c0e11ae09d97a270a80d259e05773101df62e11e9d  kali-linux-2017.1-amd64.iso
~~~

### Live CD Test

- Works fast with 2 GB memory
- No graphic issues

### Text Installer

- Text installer is fast
- IP address has to be entered manually
  - IP address details can be found in VM Settings Window
  - Networking configuration is slow -> be patient

- Setting clock takes time -> be patient
- The disk copying process takes some time
- Finding Network Mirror failed

### Troubleshooting

### Graphics error

- After installation -> Graphics error as described here:
  - https://github.com/QubesOS/qubes-issues/issues/2761
  - https://s24.postimg.org/vra4obket/IMG_0491.jpg

- LiveCD uses autologin -> no error

#### Fix

The following is assumed: 
- during the installation the username `user` was selected
- the name of the Kali Linux HVM is `kali-vm`
- the Kali Linux `.iso` file is in the `Download` directory of the `personal` VM

Enable autologin in Gdm3 config file:

1. After installation, start `kali-vm` in Dom0 terminal with:

        dom0: qvm-start kali-vm --cdrom personal:/home/user/Downloads/kali-linux-<version>-amd64.iso

    for example:
        
        dom0: qvm-start kali-vm --cdrom personal:/home/user/Downloads/kali-linux-2017.1-amd64.iso

2. Select in boot menu `LiveCD`
3. Mount the root partition of the `kali-vm` installation in `kali-vm`

        sudo mount /dev/mapper/kali--vg-root /mnt/

4. Enable automatic login by uncommenting the following lines in `/mnt/etc/gdm3/daemon.conf`

        [daemon]
        # Enable automatic login
          # AutomaticLoginEnable = true
          # AutomaticLogin = user

   like this

        [daemon]
        # Enable automatic login
          AutomaticLoginEnable = true
          AutomaticLogin = user

5. Save the file and shutdown `kali-vm`
6. Start `kali-vm` **without** the Kali Linux CD attached: `qvm-start kali-vm`


