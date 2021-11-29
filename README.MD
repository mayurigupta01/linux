**Cmpe283  Assignment 3**

Assignment is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.

•	For CPUID leaf node %eax=0x4FFFFFFD: 
 Return the number of exits for the exit number provided (on input) in %ecx. this value should be returned in %eax 
 
 
•	For CPUID leaf node %eax=0x4FFFFFFC:
          Return the time spent processing the exit number provided (on input) in %ecx.
          Return the high 32 bits of the total time spent for that exit in %ebx
         Return the low 32 bits of the total time spent for that exit in %ecx 
         
         
**Question 1**: completed as an Individual

**Question2** :Steps to complete the assignment .

•	Code Change: The code changes have been made in arch/x86/kvm/cpuid.c and arch/x86/kvm/vmx/vmx.c files.

•	Build the linux kernel(Implementation on Hypervisor)
 
   o	cd linux 
 
   o	make -j 6 modules

   o	make -j 6

 <img width="418" alt="image" src="https://user-images.githubusercontent.com/89235391/143798659-ba0a671c-4db7-430f-b135-e9b83e5d7097.png">

 
o	create a new installer

o	sudo make INSTALL_MOD_STRIP=1 modules_install

o	sudo make install

o	sudo rmmod kvm_intel

o	sudo rmmod kvm 

o	sudo modprobe kvm -(Loads the Kvm)

o	sudo modprobe kvm_intel (Loads the Kvm_intel)

o	lsmod | grep kvm

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798670-3654690d-a25c-4a85-86c8-0678d0769a67.png">


•	Test the code on nested Virtual machine.

  o	Nested VM is created as a part of assignment2.
  
  o	On the nested VM run the following commands
  
  o	 To test the total exit per reason , cpuid -l 0x4FFFFFFD -s 0
  
  o	 To test CPU cycles spent to handle the exit, cpuid -l 0x4FFFFFFC -s 0 

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798681-fbd2073f-0915-4ddc-b00a-3384670f9fe4.png">

 
 o	On Hypervisor, run dmesg
 

<img width="409" alt="image" src="https://user-images.githubusercontent.com/89235391/143798692-bd603022-4594-4427-9c48-d25dc9565773.png">

<img width="373" alt="image" src="https://user-images.githubusercontent.com/89235391/143798715-3a2b7174-5035-48fe-9fc6-f19f8db5f5d0.png">
          
 
o	On Hypervisor, run dmesg

<img width="368" alt="image" src="https://user-images.githubusercontent.com/89235391/143798770-1866f93f-2e51-42e6-b9c7-c04ad48190fd.png">

<img width="389" alt="image" src="https://user-images.githubusercontent.com/89235391/143798784-8956a718-a3d8-43f0-ae61-57ff47bcdd67.png">

 
**Question 3** - 
Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail? 


The frequency of all exits does not grow very rapidly. As per my observance I see that exit 1 (external interrupt) grew quite rapidly from 33527 to 144716. There are many exits code that were exited 0 times. (e.g. 13 to 27, 50 to 53 , 55-60) 
Approximately how many exits does a full VM boot entail?

After VM Reboot 


<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798841-ccce7ae1-8f2b-4904-84ab-e59149a0b100.png">

 
**Question 4 **- Of the exit types defined in the SDM, which are the most frequent? Least?

Most Frequent exits:

•	1 (External interrupt)

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798889-a81bb2b4-7c7c-4d50-b861-44b8dda22dc9.png">

•	12(HLT) 

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798904-204b0c8c-617c-4625-aec8-43dfcd492313.png">

•	30 (I/O Instructions)

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798913-0f6a23e4-1aed-41e1-8199-642d1890b3b8.png">

•	32 (WMRSR)

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798934-3ea54d5b-341a-4d15-ae1a-e12dfc94ee65.png">

•	48 (EPT Violation)

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798947-e400fc9a-ce2a-4b3b-8681-54cc915ee350.png">

•	49(EPT Misconfiguration)
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798954-33e29bd8-d674-45bc-acfb-4bb2864c904b.png">

Least Frequent exits:
•	MOV DR

 <img width="450" alt="image" src="https://user-images.githubusercontent.com/89235391/143798971-3962f1b8-0df5-4ecc-ba95-10dc638ce860.png">


•	WBINVD

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143798989-71bbfc2f-c20e-4f3e-9cc9-245bc1cf6790.png">


•	PAUSE

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143799005-3cb6d33e-76c8-4883-b938-42dbf92b2312.png">

 
**•	Valid exit but not enabled in KVM:**
  3,4,5,6,11,16,17,33,34 ,51,63,64,66,68,69
![notexistinSDM](https://user-images.githubusercontent.com/89235391/143799048-e62ed5d2-175d-40a9-92d7-68e28fca9541.png)

![notenabledinkvm](https://user-images.githubusercontent.com/89235391/143799064-73946d4c-f41c-4c60-9fc5-35b87569b908.png)




  
