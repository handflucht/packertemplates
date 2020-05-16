# First tries with packer



# Installation
## Windows 10
Just download the binary from http://www.packer.io.
## In WSL on Windows 10
Trying to run `packer` in WSL after installation let to several problems. For that reasons, this plan has been canceled.
   
# Using Packer
## General
We need two files:
* packer-json 
  <br/>file to provide main information for packer and about the image to build
* ks.cfg
  <br>"answer-file" of the installer of the OS
  
Important information:
* if `ssh_username` and `ssh_password` is used, make sure a user with this password is defined in the kickstart-file.
* depending on the available resources, you can or must modify the value `ssh_timeout` - the time after which packer can login to the new installed system via ssh. 
* `http_directory` defines the directory where the kickstart-file is placed. This will create a http-server with a free port to provide the kickstart-file

## Build image
```powershell
S C:\github\handflucht\packertemplates\templates\CentosIsoToVirtualbox> .\packer.exe build .\CentosIsoToVirtualbox.json
virtualbox-iso: output will be in this color.

==> virtualbox-iso: Retrieving Guest additions
==> virtualbox-iso: Trying C:\Program Files\Oracle\VirtualBox/VBoxGuestAdditions.iso
==> virtualbox-iso: Trying file://C:/Program%20Files/Oracle/VirtualBox/VBoxGuestAdditions.iso
==> virtualbox-iso: file://C:/Program%20Files/Oracle/VirtualBox/VBoxGuestAdditions.iso => C:/Program Files/Oracle/VirtualBox/VBoxGuestAdditions.iso
==> virtualbox-iso: Retrieving ISO
==> virtualbox-iso: Trying file://C:/Downloads/CentOS-8.1.1911-x86_64-dvd1.iso
==> virtualbox-iso: Trying file://C:/Downloads/CentOS-8.1.1911-x86_64-dvd1.iso?checksum=md5%3A8d0573c5fb5444007936b652d8c6724d
==> virtualbox-iso: file://C:/Downloads/CentOS-8.1.1911-x86_64-dvd1.iso?checksum=md5%3A8d0573c5fb5444007936b652d8c6724d => C:/Downloads/CentOS-8.1.1911-x86_64-dvd1.iso
==> virtualbox-iso: Starting HTTP server on port 8542
==> virtualbox-iso: Creating virtual machine...
==> virtualbox-iso: Creating hard drive...
==> virtualbox-iso: Creating forwarded port mapping for communicator (SSH, WinRM, etc) (host port 2451)
==> virtualbox-iso: Executing custom VBoxManage commands...
    virtualbox-iso: Executing: modifyvm CentOs8_2 --memory 8192
    virtualbox-iso: Executing: modifyvm CentOs8_2 --cpus 4
==> virtualbox-iso: Starting the virtual machine...
==> virtualbox-iso: Waiting 10s for boot...
==> virtualbox-iso: Typing the boot command...
==> virtualbox-iso: Using ssh communicator to connect: 127.0.0.1
==> virtualbox-iso: Waiting for SSH to become available...
==> virtualbox-iso: Connected to SSH!
==> virtualbox-iso: Uploading VirtualBox version info (6.1.6)
==> virtualbox-iso: Uploading VirtualBox guest additions ISO...
==> virtualbox-iso: Gracefully halting virtual machine...
==> virtualbox-iso: Preparing to export machine...
    virtualbox-iso: Deleting forwarded port mapping for the communicator (SSH, WinRM, etc) (host port 2451)
==> virtualbox-iso: Exporting virtual machine...
    virtualbox-iso: Executing: export CentOs8_2 --output output-virtualbox-iso\CentOs8_2.ovf
==> virtualbox-iso: Deregistering and deleting VM...
Build 'virtualbox-iso' finished.

==> Builds finished. The artifacts of successful builds are:
--> virtualbox-iso: VM files in directory: output-virtualbox-iso
PS C:\github\handflucht\packertemplates\templates\CentosIsoToVirtualbox> ls .\output-virtualbox-iso\


    Verzeichnis: C:\github\handflucht\packertemplates\templates\CentosIsoToVirtualbox\output-virtualbox-iso


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       16.05.2020     22:19     1858640384 CentOs8_2-disk001.vmdk
-a----       16.05.2020     22:17           6390 CentOs8_2.ovf
```


# Good to know

* Values for `guest_os_type`
 
  Possible values for `guest_os_type` (if using VirtualBox) can be listed by executing `VBoxManage list ostypes`. More Information here: https://www.packer.io/docs/builders/virtualbox-iso.html#guest_os_type.
  
* Even if often listed explicitly, the value for `ssh_port` by default is `22`.
