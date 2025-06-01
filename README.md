# my-Mod-OS
ModOS: Custom Operating System with Selective Feature Disabling for Controlled and Secure  Execution Environments   
Complete Guide: Compile and Install Linux Kernel 6.12.13-custom on Kali Linux (with OS Prober Disabled)



1. Install Required Dependencies

sudo apt update
sudo apt install build-essential libncurses-dev bison flex libssl-dev libelf-dev bc git wget



2. Download Kernel Source (v6.12.13)

cd /usr/src
sudo wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.12.13.tar.xz
sudo tar -xf linux-6.12.13.tar.xz
cd linux-6.12.13



3. Copy Existing Kernel Config

sudo cp -v /boot/config-$(uname -r) .config



4. Set Custom Version Suffix

Edit the Makefile:

sudo nano Makefile

Find the line:

EXTRAVERSION =

Change it to:

EXTRAVERSION = -custom

Or set it using environment variable during build:

export LOCALVERSION=-custom

This ensures the kernel name becomes 6.12.13-custom.



5. Optional: Customize Kernel Config

sudo make menuconfig      # Terminal UI


6. Build Kernel and Modules

sudo make -j$(nproc)
sudo make modules -j$(nproc)



7. Install Kernel Modules

sudo make modules_install



8. Install Kernel

sudo make install

This installs:

/boot/vmlinuz-6.12.13-custom

/boot/initrd.img-6.12.13-custom (if created)

/boot/System.map-6.12.13-custom


GRUB entries are also created.



9. Disable OS Prober in GRUB

sudo nano /etc/default/grub

Add or modify this line:

GRUB_DISABLE_OS_PROBER=true

Save and exit.



10. Regenerate initramfs (if needed)

sudo update-initramfs -c -k 6.12.13-custom



11. Update GRUB

sudo update-grub



12. (Optional) Set Custom Kernel as Default

sudo grub-set-default 0



13. Reboot and Confirm

sudo reboot

After reboot:

uname -r

You should see:

6.12.13-custom
