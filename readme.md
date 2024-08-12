Compiling linux kernel and create an ISO file

1. Compile the kernel source
```bash
	wget https://cdn.kernal.org/pub/linux/kernel/v6.x/linux-6.x.tar.xz
	tar -xf linux-6.x.tar.xz
	cd linux-6.x
```

2. Configure the Kernal (configure the kernal options according to your hardware or preferences.)
```bash
	make menuconfig
```

3. Compile the Kernal
```bash
	make -j$(nproc)
```

4. Install Kernal Modules:
```bash
	sudo make modules_install
```

5. Install the Kernal
```bash
	sudo make install
```

Setup the Root Filesystem

1. Create a directory for the Root Filesyste
```bash
	mkdir -p rootfs/{bin,sbin,etc,proc,sys,usr,var,lib}
```

2. Install BusyBox (or other essential binaries):
```bash
	cd busybox-1.x
	make menuconfig
	make -j$(nproc)
	sudo make install
```

3. Copy Kernal Modules
```bash
	sudo cp -r /lib/modules/${uname -r) rootfs/lib
```

4. Create Device Nodes
```bash
	sudo mknod -m 600 rootfs/dev/console c 5 1
	sudo mknod -m 666 rootfs/dev/null c 1 3
```

5 Congigure init scripts
Create an init file under rootfs/etc to deine the startup process.

Create the ISO File

1. Craete a bootable ISO Directory Structure
```bash
	mkdir iso
	cp -r rootfs iso/
	mkdir -p iso/boot/grub
```

2. Copy Kernel and Initrd
```bash
	cp path/to/compiled/kernal iso/boot/vmlinuz
	cp path/to/initrd.img iso/boot/initrd.img
```

3. Create GRUB Configuration File
Create 'iso/boot/grub/grub.cfg' with following content
```bash
	set timeout=10
	menuentry "My Custome Linux" {
		linux /boot/vmlinuz root=/dev/sda1
		initrd /boot/initrd.img
	}
```

4. Create ISO File
```bash
	grub-mkrescue -o my_custom_linux.iso iso/
```
