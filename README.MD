•	**Configure Linux based virtual machine** 
  -	Installed VMware fusion
  -	Configured Ubuntu (64-bit 20.04.3) with 16 Gb RAM, 6 CPU’s and 100 GB storage
  -	run the below commands on the VM.

•	**Install git **
  -	Sudo apt-get install git 

•	**Download the Linux Kernel Source code**
  -	git clone https://github.com/mayurigupta01/linux.git (forked the https://github.com/torvalds/linux to my GitHub account.

•	**Go to Linux directory** 
  - Cd linux/

•	**run the following commands to build the Linux kernel source code**.
   - sudo apt-get install libncurses-dev
   - sudo apt-get install libssl-dev
   - sudo apt install flex
   - sudo apt install bison 
   - sudo apt-get install gcc 
   - make menuconfig
   - uname -a
   - cp /boot/config-5.11.0-40-generic .config
   - make oldconfig
   - make prepare - failed at first time.
   - sudo apt install libelf-dev
   - make prepare
   - make
   - make clean 
   - make -j 6 modules
   
   **Failed due to certificate issues .**
   ****Followed below troubleshoot steps **:**
   openssl req -x509 -newkey rsa:4096 -keyout certs/mycert.pem -out certs/mycert.pem -nodes -days 3650
   
   - Added to the .config file.
   <img width="468" alt="Picture1" src="https://user-images.githubusercontent.com/89235391/141537165-c7896ea0-6a26-474e-90ac-4eb4659391c4.png">

   - make -j 6 modules
     Trobleshoot steps after first failure on building linux kernnel 
   - sudo apt install dwarves
   - make -j 6 modules
   - make -j 6
   - sudo make INSTALL_MOD_STRIP=1 modules_install
   - sudo make install
   - sudo reboot
   - uname -a - (Linux kernel changed to 5.15 )
   
   <img width="574" alt="KernelReady" src="https://user-images.githubusercontent.com/89235391/141539655-0efe3a82-21e5-4864-84cc-40f9839b2458.png">
   
   
 •	**Create a.c file that will be used to find the capabilities of the  MSR's**
   - vim cmpe283-1.c 
      File contents/code click here 
     
 •	**Create new kernel module for MSRs**
   - MakeFile 
   - File Content
     obj-m += cmpe283-1.o

     all:
		          make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

     clean:
		          make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
     
    - cd linux/cmpe283_1/
    - make
              
  •	**Create new kernel module for MSRs**        
    - sudo insmod cmpe283-1.ko    
    
  •	**Verify the mesaage output from the kernel in system log**    
    - dmesg 
    - rmmod cmpe283-1
      
      
   <img width="709" alt="MSR" src="https://user-images.githubusercontent.com/89235391/141539924-e9b1125c-5950-4b5f-bba6-f7c0cf1788c2.png">

  •	**Verify the other files in project directory** 
    - virtual-machine:~/code# ls
      cmpe283-1.c  cmpe283-1.ko  cmpe283-1.mod  cmpe283-1.mod.c  cmpe283-1.mod.o  cmpe283-1.o  linux  Makefile modules.order  Module.symvers 
   
   <img width="1257" alt="files" src="https://user-images.githubusercontent.com/89235391/141539991-78fdf2db-fd27-402e-a7c6-92ca2b749eb2.png">
   
    
  •	**Verify the Output** 
   - Link to output 
   
  •	**Commit changes to GitHub** 
   - cp MakeFile and cmpe283-1.c to linux/cmpe283
   - cd linux/cmpe283
   - git add MakeFile cmpe283-1.c
   - git commit -m "Initial Commit - This is my cmpe283 Assignment1"
   - git push 
  
