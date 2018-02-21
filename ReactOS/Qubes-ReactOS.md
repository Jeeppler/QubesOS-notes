# ReactOS

## ReactOS + Qubes OS

ReactOS images can be found on SourceForge: https://www.reactos.org/

ReactOS offers six different download options:

- `ReactOS-<version>-REL-VMware.zip` - Image for the VMware desktop virtualization solution
- `ReactOS-<version>-REL-vbox.zip` - Image for the VirtualBox desktop virtualization solution
- `ReactOS-<version>-REL-QEMU.zip` - Image for the QEMU emulator
- `ReactOS-<version>-live.zip` - LiveCD and installer ISO
- `ReactOS-<version>-iso.zip` - Unable to create domain -> throws libxenlight error 
- `ReactOS-<version>-src.zip` - The source code of ReactOS for the specific version. 

### LiveCD 

1. Download LiveCD and unzip it

2. Create a new HVM named reactOS

3. Start 
~~~
qvm-start reactOS --cdrom personal:/home/user/Downloads/ReactOS-<version>-live/ReactOS-<version>-Live.iso
~~~

example for version 0.4.5:

~~~
qvm-start reactOS --cdrom personal:/home/user/Downloads/ReactOS-0.4.5-live/ReactOS-0.4.5-Live.iso
~~~

#### Issues:

LiveCD:

- Mouse pointer position is not synchronized with Dom0 mouse pointer
- LiveCD mode -> mouse clicks in main menu don't work
- Installer crashes

### Virtualbox Image

1. Download VirtualBox image and unzip it (in DomU)

2. Convert the image (in DomU)

~~~
# in AppVM data-os
qemu-img convert ReactOS.vhd -O raw reactos.img
~~~

3. Copy it to Dom0 (in Dom0)
  - Note: the VBox has a dynamic size of 20GB, the complete image will be copied over
  
~~~
# in Dom0
qvm-run --pass-io data-os 'cat /home/user/Downloads/reactos.img' > reactos.img
~~~

4. Create new HVM + Standalone VM `reactos` (in Dom0)

5. Move `reactos.img` to `/var/lib/qubes/appvms/reactos` (in Dom0)

6. Delete `root.img` in `/var/lib/qubes/appvms/reactos` (in Dom0)

7. Rename `reactos.img` to `root.img` (in Dom0)

~~~
cd /var/lib/qubes/appvms/reactos
mv reactos.img root.img
~~~

8. Start VM:

~~~
qvm-start reactos
~~~

#### Tested:

- Internet connection: works out of the box
- Sound: does not work
- Playing videos youtube: works well in firefox
- USB Pass-through: does now work (author's computer don't have Intel VT-d)
- Window resizing in ReactOS works
- Microphones cannot be attached

- Games: 
  - Pingus (works)
  - Age of Empires Demo (works)
  - Anno 1602 Demo (works)
  - Stronghold Demo (works well)
  - Cossacks European Wars (mouse does not work)
  - Empire Earth Demo (memory error)
  - Soldier of Fortune 2 Demo (error needs OpenGL)
  - Praetorians Demo (does not work)
  - Robin Hood - The Legend of Sherwood Demo (installation: works, game hangs at start screen)
  - Wild Life Park Demo (installer does not work -> does not recognize language)
  - GTA 2 Demo (installer errors, fails afterwards)
  - Stronghold Crusader Multiplayer Demo (works well)
  - Age of Empires II: Age of Kings Demo (installation: no sound card warning, game works)
  - Battlefield 1942 Multiplayer Demo (installer hangs)

- PDF reader:
  - Adobe Reader 11 (Connection fails during installation)
  - Adobe Reader 9 does not work (rendering issues)
   
- Haxe Development: FlashDevelop
  - Requires at least .Net 3.5
    - Can be downloaded (XP version)
    - Installation of .Net 3.5 fails
    
- VoIP: 
  - Skype: Has problems while installing and running (Skype 7.3.6)
  
- Reverse Engineering:
  - IDA 5.0 Freeware (works)

- Compression:
  - PeaZip works

- Multimedia:
  - VLC (works)
  - iTunes 12.1.3.6 32-bit (works until "registering modules" phase in the installation)
  
- Password Safe:
  - Keepass 2.3 (error during installation)
  - Keepass 1.3 (works)

- Other:
  - Skydemon 3.8.1 (installer: works, application can not be started)
    - Requires .NET Framework 2.0 -> can be found in ReactOS Application repo

- 512 MB RAM are enough to run ReactOS smooth
- Startup and shutdown are fast

#### Issues:

- Mouse pointer position is not synchronized with Dom0 mouse pointer
  - Solution: Move pointer out of VM window and than back in until both mouse pointer are above each other (looks like synchronized)
  
- Stops working sometimes
  - It seems to have problems while writing to disk
  
- IO is very CPU itensive

- VLC is unable to work with spaces in directory names -> throws errors in log

#### Xen PV Driver

https://www.xenproject.org/developers/teams/windows-pv-drivers.html

- all driver installed without showing any visible error
- can't load drivers xenbus.sys and xenfilt.sys -> system start

However, I don't know how to test if the device driver are working.



  
