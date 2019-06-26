# Step 0 - Make a working directory

mkdir ~/kernel && cd ~/kernel

# Step 1 - Download kernel and patch

wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.0/patch-5.0.21-rt13.patch.gz

wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.0.21.tar.gz

  #x - extract

  #z - pipe through gunzip

  #v - verbose (text output)

  #f - from file

tar -xzvf linux-3.18.11.tar.gz

# Step 3 - Patch the kernel

cd linux-3.18.11

  #c - pipe file contents to stdout

  #d - decompress

gzip -cd ../patch-5.0.21-rt13.patch.gz | patch -p1 --verbose

# Step 4 - Enable realtime processing

  This step requires libncurses-dev

sudo apt-get install libncurses-dev

make menuconfig

  General setup ---> [Enter]
  
  Preemption Model (Preemption Model (Fully Preemptible Kernel (RT)) [Enter]
  
  (X) Fully Preemptible Kernel (RT) [Enter] #Select

  Exit

  Kernel hacking --> [Enter]

  Memory Debugging [Enter]

  Check for stack overflows #Already deselected - do not select

  Exit

  <Save> [Enter]

  .config

  <Okay> [Enter]

  <Exit> [Enter]
 
 # Step 5 - Compile the kernel
 
 make -j4
 
# Step 6 - Make modules & install
 
sudo make modules_install -j4

sudo make install -j4

# Step 7 - Verify and update

  Verify that initrd.img-5.0.21-rt13, vmlinuz-5.0.21-rt13, and config-5.0.21-rt13 exist. They should have been created in the previous step.

Code:

cd /boot

ls

  Update grub 

sudo update-grub

# Step 8 - Reboot and enjoy!

  Reboot the computer, and when the grub menu appears during boot, select the newly created RT kernel.

sudo reboot

  Once rebooted, you can verify that everything was successful by doing:

uname -a

  Output looks like:

  Linux ...-home 5.0.21-rt13 #1 SMP PREEMPT RT ... GNU/Linux

  Note the "SMP PREEMPT RT".
