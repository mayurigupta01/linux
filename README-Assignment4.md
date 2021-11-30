Assignment 3 – completed (prerequisite)


**Question1 **: completed as an individual



**Question2 **:Steps to complete assignment 

With nested paging – 
1.	Inside Inner VM run the sequence of 0X4FFFFFFD and below is the count of total exits for each exit reason defined in KVM

With EPT

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143968304-4ff2a8a7-0db7-4eae-a649-8e70effe4a89.png">

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143968319-ab3100f9-764b-48ae-a926-6f05495296f3.png">

<img width="435" alt="image" src="https://user-images.githubusercontent.com/89235391/143968328-585dc420-0923-4c6b-8376-6f824a4e7262.png">


2.	Shutdown your test (inner) VM. 

Completed 


3. 	Remove the ‘kvm-intel’ module from your running kernel: 

  <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143968633-f105f943-3c4c-4c28-8cad-0f1ddaa51155.png">


  rmmod kvm-intel

  <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143968654-8fa7a597-1b28-4c7d-b68b-caa850dd8b91.png">
  
  
  4.	Reload the kvm-intel module with the parameter ept=0 (this will disable nested paging and forceKVM to use shadow paging instead) 
       insmod /lib/modules/XXX/kernel/arch/x86/kvm/kvm-intel.ko ept=0 

  <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143968693-377a505a-bfae-42cd-af7d-688e9378bdd9.png">
  
  5.	Boot the same VM again and capture the total count for each type of exit handled by KVM.
      
      without EPT 
      
   <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143968730-bf176dd9-fe58-4598-ad5d-b45870e624fe.png">
      
   <img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143968746-53960515-f438-49f9-b78d-d6a209290440.png">
   
      
  **Question3: What did you learn from the count of exits? Was the count what you expected? If not, why not?
  
   I observed that after without EPT(shadow paging) resulted in increased count of exits versus nested paging (with EPT).
   So nested paging is more faster in terms of reading page walk hence results in less exits .With shadow paging enabled this count increased a lot.
   
   <img width="448" alt="image" src="https://user-images.githubusercontent.com/89235391/143968926-9af5385e-1801-416d-a5d5-e18a7727104a.png">


**Question4: What changed between the two runs (ept vs no-ept)? **

With EPT (14 ==0 , 33==0  )

There were no exits for reason 14 and 33.

With out EPT

INVLPG. Guest software attempted to execute INVLPG

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143968977-5a085d68-4681-475c-8f66-51bfe008e0d4.png">

VM-entry failure due to invalid guest state. 

<img width="468" alt="image" src="https://user-images.githubusercontent.com/89235391/143969019-ae178cd2-dd5c-40d7-9de5-a93ea133f593.png">

Also noticed that nested VM start had to be done from recovery mode and it did not start safely as well as it took lot more time.
Whole nested VM performance is very slow in shadow paging as compared to nested paging.






   
   







