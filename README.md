## PS1TD rEFInd theme

[rEFInd](http://www.rodsbooks.com/refind/) is an easy to use boot manager for UEFI
based systems. This is a custom theme for it.
This theme has support for tool icons(memtest, shell and so on),
indicators for media that is external(usb, web, cd),
and support for custom fonts

![rEFInd PS1TD](http://i.imgur.com/3bMG6U7.png)

### Usage

1.  Locate your refind EFI directory. This is commonly `/boot/EFI/refind`
    though it will depend on where you mount your ESP and where rEFInd is
    installed. `fdisk -l` and `mount` may help.

```
sudo su
```

```
cd /boot/efi/EFI/refind
```

2.  Create a folder called `themes` inside it, if it doesn't already exist

```
mkdir themes
```

```
cd themes
```

3.  Clone this repository into the `themes` directory.

```
git clone https://github.com/PS1TD/rEFInd-PS1TD
```

4.  To enable the theme add `include themes/rEFInd-PS1TD/theme.conf` at the end of
    `refind.conf`.

```
cd ..
```

```
echo "include themes/rEFInd-PS1TD/theme.conf" >> refind.conf
```

### Hide or Add Entries

There are many ways of hiding or adding unwanted entries.

In theme.conf there is an included way to ignore directories.

```
#dont_scan_dirs ESP:/EFI/boot,EFI/Dell,EFI/memtest86
#dont_scan_dirs EFI/systemd,EFI/kali,EFI/Pop_OS-9a3d833c-29a5-4e4e-9dd6-ca48ddf4f94e,EFI/Boot
```

For more information read through refind.conf

### Adding Shell

Download and unzip [ShellBinPkg.zip](https://github.com/tianocore/edk2/releases/download/edk2-stable201911/ShellBinPkg.zip).

```
unzip ShellBinPkg.zip
```

Select the shell you want and move it to EFI/tools directory.

```cp ShellBinPkg/UefiShell/X64/Shell.efi /boot/efi/EFI/tools/shell.efi
sudo cp ShellBinPkg/UefiShell/X64/Shell.efi /boot/efi/EFI/tools/shell.efi
```

The shell.efi should be automatically detected by rEFInd.

### Adding Memtest

Download and unzip [MemTest86](https://www.memtest86.com/download.htm).

```
unzip memtest86-usb.zip
```

Follow instructions from [guide](https://www.memtest86.com/tech_configuring-grub.html).

```
mkdir memtest
```

Check sectors.

```
fdisk -lu memtest86-usb.img
```

Multiply start sector of the EFI System sector by 512. (514048\*512=263192576)

```
sudo mount -o loop,offset=263192576 memtest86-usb.img memtest
```

Copy /EFI/BOOT/ into /boot/efi/EFI/tools/memtest/

```
sudo cp -r memtest/EFI/BOOT/. /boot/efi/EFI/tools/memtest/
```

MemTest86 should be automatically detected by rEFInd.

### Custom Entries

Here's an example of custom menuentry configuration (from the screenshot)

```nginx
menuentry "Arch Linux" {
	icon /EFI/refind/themes/rEFInd-PS1TD/icons/os_arch.png
	loader vmlinuz-linux
	initrd initramfs-linux.img
	options "rw root=UUID=dfb2919d-ff78-48db-a8a7-23f7542c343a loglevel=3"
}

menuentry "Windows" {
	icon /EFI/refind/themes/rEFInd-PS1TD/icons/os_win.png
	loader /EFI/Microsoft/Boot/bootmgfw.efi
}

menuentry "OSX" {
	icon /EFI/refind/themes/rEFInd-PS1TD/icons/os_mac.png
	loader /EFI/Apple/Boot/bootmgfw.efi
}
```

Entries that are autodetected should also show the proper icons.

### Background sizes

If you find the background looks blurry it may be due to the included wallpaper
being an incorrect resolution for your monitor. You can download the [original
high quality wallpaper][wallpaper], resize it as appropriate, and replace the
`background.png`.

You can of course also choose your own background!

### Attribution

The Theme is a modified [rEFInd-minimal][refind-minimal] theme by [evanpurkhiser][refind-minimal-author].

Icons are from [Lightness for burg][icons] by [SWOriginal][icon-author].

The background is [Minimalist Wallpaper][wallpaper] by
[LeonardoAIanB][wallpaper-author].

[refind-minimal]: https://github.com/evanpurkhiser/rEFInd-minimal
[refind-minimal-author]: https://github.com/evanpurkhiser
[icons]: http://sworiginal.deviantart.com/art/Lightness-for-burg-181461810
[icon-author]: http://sworiginal.deviantart.com/
[wallpaper]: http://leonardoalanb.deviantart.com/art/Minimalist-wallpaper-295519786
[wallpaper-author]: http://leonardoalanb.deviantart.com/
