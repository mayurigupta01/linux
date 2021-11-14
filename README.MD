Cmpe 283 Assignmemt 2 :

Your assignment is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.
1.  For CPUID leaf node %eax=0x4FFFFFFF:
◦ Return the total number of exits (all types) in %eax

2.  For CPUID leaf node %eax=0x4FFFFFFE:
◦ Return the high 32 bits of the total time spent processing all exits in %ebx
◦ Return the low 32 bits of the total time spent processing all exits in %ecx

Question 1: Individual

**Question 2: Steps to complete the assignment **

**Implementation on Hypervisor**

- Implemented the CPUID leaf node functionality in vmx.c and cpuid.c files .
- Run following command on Ubuntu desktop running as hypervisor.
- make -j 6 modules
- make -j 6
- sudo make INSTALL_MOD_STRIP=1 modules_install
- sudo make install
- sudo rmmod kvm_intel
- sudo rmmod kvm
- lsmod | grep kvm  - (no Kvm found)
- sudo modprobe kvm -(Loads the Kvm)
- sudo modprobe kvm_intel (Loads the Kvm_intel)
- lsmod | grep kvm 

<img width="262" alt="image" src="https://user-images.githubusercontent.com/89235391/141673031-844d4c9a-6b5b-41e2-8357-1f6df825401b.png">

**Create a Nested Virtual Machine**

- grep -Eoc '(vmx|svm)' /proc/cpuinfo  -returns 12 (CPU supports nested hardware  virtualization)
- Followed the link to create a Kvm inside the Vm (https://wiki.qemu.org/Hosts/Linux)
- On the nested VM run the following commands.
- Sudo apt Install cpuid
- Run cpuid -l 0x4FFFFFFF

 <img width="768" alt="Screen Shot 2021-11-14 at 12 15 54 AM" src="https://user-images.githubusercontent.com/89235391/141673235-39debe2c-d1a3-4742-8078-e6187fce87c5.png">
 
 verify the Output on hypervisor :
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/141673242-f40da944-68cb-457f-8784-5d5153726171.png">

 - Run cpuid -l 0x4FFFFFFE

  <img width="748" alt="Screen Shot 2021-11-14 at 12 17 51 AM" src="https://user-images.githubusercontent.com/89235391/141673278-da3bd609-3304-4a0d-ba5d-7cc8e2579ec4.png">

   verify the Output on hypervisor :
   
   <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/141673297-58baacb3-dbb0-48b0-b02a-9e4afb33ecb0.png">


 ** Challenges faced during creating the VM inside the VM with Mac os on Host Machine.**
  
  - sudo apt update
  - sudo apt install cpu-checker
  - kvm-ok
  - sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
  - sudo systemctl is-active libvirtd
** ** Error on launching the VM  , after troubleshoot figured out that qemu-Kvm is using the old version and sudo apt 
    upgrade does not install the lattest version of qemu.****
    
    To fix the issue followed the -	https://wiki.qemu.org/Hosts/Linux and created a VM .
    Launched VM "./build/x86_64-softmmu/qemu-system-x86_64 -m 4096 -enable-kvm -drive if=virtio,file=test.qcow2,cache=none -cdrom /home/mayuri/Desktop/ubuntu-20.04.3-desktop-amd64.iso -vnc :1
    " 
    On vnc viewer , used nested VM .
    
    
