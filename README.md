# Assignment-3
Below are the steps followed to complete Assignment 3.  
Note: I have used the VM created for Assignment 2. As a result, it has all the packages installed for cpuid, qemu and ubuntu image.  
1) Made changes to cpuid.c and vmx.c files as mentioned in the PDF for 0x4FFFFFFE and 0x4FFFFFFF  

2) Run  
make -j 8 modules  
sudo bash    
make INSTALL_MOD_STRIP=1 modules_install   
![Screenshot 2022-12-12 011831](https://user-images.githubusercontent.com/45283425/207023126-4d87fde7-b42b-4b1d-baa9-0a2a461395ef.png)  
![Screenshot 2022-12-12 011901](https://user-images.githubusercontent.com/45283425/207023263-57dcd375-d2d6-46ba-ba87-c9b1b2332e70.png)  

3) Remove existing kvm modules and reload the kvm modules again.    
sudo rmmod kvm_intel  
sudo rmmod kvm  
sudo modprode kvm  
sudo modprobe kvm_intel  
![Screenshot 2022-12-12 012358](https://user-images.githubusercontent.com/45283425/207023515-59a634b5-298e-44f7-b20c-57bf0bba66df.png)  

4) Run the below command to run ubuntu image.    
sudo qemu-system-x86_64 -enable-kvm -hda bionic-server-cloudimg-amd64.img -drive "file=user-data.img,format=raw" -m 512 -curses -nographic     
Now you will be redirected to the New Terminal of Nested VM. Login by providing the username as ubuntu and password as akhil.  
![Screenshot 2022-12-12 012821](https://user-images.githubusercontent.com/45283425/207024624-8052359d-20f4-4c39-bd40-275b151fdc4d.png)  
![Screenshot 2022-12-12 012925](https://user-images.githubusercontent.com/45283425/207024695-088c384a-bbba-4e0e-8985-8dba297a28b8.png)  

5)  Write a test script inside test_cpuid.sh  
#!/bin/bash  
for i in {0..76}   
do  
    echo "EXIT $i"  
    cpuid -l $1 -s $i  
done  

6) Now test the functionality for 0x4ffffffe.  
![Screenshot 2022-12-12 013741](https://user-images.githubusercontent.com/45283425/207026765-465fcd69-9e85-4288-a145-e8167f610eec.png)  
![Screenshot 2022-12-12 013852](https://user-images.githubusercontent.com/45283425/207027492-56dfbe21-8684-48d7-a078-5190f83f8d36.png)  
![Screenshot 2022-12-12 013928](https://user-images.githubusercontent.com/45283425/207027891-b18b5f56-9aa3-47c9-ad13-1b91818ff25a.png)  

7) Now test the functionality for 0x4fffffff.  
![Screenshot 2022-12-12 013809](https://user-images.githubusercontent.com/45283425/207028968-6c98b08f-849a-4327-9200-dc09309e8c57.png)  
![Screenshot 2022-12-12 025629](https://user-images.githubusercontent.com/45283425/207028753-e022a83f-8caa-4019-91d5-a01b7ca210a4.png)  
![Screenshot 2022-12-12 025657](https://user-images.githubusercontent.com/45283425/207028814-cdf9e62c-d310-4f9a-9d5e-d4cdeda86915.png)  

# Question 3:  
Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there 
more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?  
Ans: The number of exits increased at a stable rate for exit numbers 10 and 12.  
![Screenshot 2022-12-12 015628](https://user-images.githubusercontent.com/45283425/207030751-4f4bf55c-f7d8-4215-8a02-6ca1953c30b2.png)  
![Screenshot 2022-12-12 015747](https://user-images.githubusercontent.com/45283425/207030877-a9c7a28e-a254-4fc6-b751-6bf1f4e561f5.png)  

# Question 4:  
Of the exit types defined in the SDM, which are the most frequent? Least?  
Ans: Most Frequently called exits- 30(IO_INSTRUCTION), 49(EPT_MISCONFIG),28(CR_ACCESS)  
     Least Frequently called exits- 29( DR_ACCESS), 54(WBINVD), 0(EXCEPTION_NMI)  





# Assignment-2
Below are the Steps followed to completed Assignment 2.

1) Created a Virtual Machine in GCP enabling nested Virtualization. 
![Screenshot (27)](https://user-images.githubusercontent.com/45283425/205819703-1aace470-c427-4548-a229-b82d4bac2155.png)

2) Forked the Git repository from https://github.com/torvalds/linux and cloned into into the GCP instance.

3) Built the kernel by following the steps specified in the video. After performing all the steps- The new kernel version is booted.
![Screenshot 2022-12-04 134454](https://user-images.githubusercontent.com/45283425/205821449-d197c6fe-8c99-47e9-8349-f7128e527392.png)

4) Made changes to cpuid.c and vmx.c files as mentioned in the video for Assignment 2.

5) Now Run these commands to build and install modules.  
   make -j 8 modules  
   sudo bash  
   make INSTALL_MOD_STRIP=1 modules_install  
   ![Screenshot 2022-12-05 194150](https://user-images.githubusercontent.com/45283425/205825461-6fc478d1-0d6b-4707-8e8b-dfcb48d3f755.png)

   
6) Remove existing kvm modules and reload the kvm modules.  
   sudo rmmod kvm_intel  
   sudo rmmod kvm  
   sudo modprode kvm  
   sudo modprobe kvm_intel  
   ![Screenshot 2022-12-05 194225](https://user-images.githubusercontent.com/45283425/205825829-fd6e5583-53da-4cb2-80a4-9e5a93678959.png)
7) Below are the steps involved to test the functionality.  
  Download cloud image and install qemu packages.  
  wget https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img  
  sudo apt update && sudo apt install qemu-kvm -y  
  
8) Now change the password for the ubuntu cloud image.   
   a)sudo apt-get install cloud-image-utils    
   
   b) cat >user-data <<EOF     
    #cloud-config     
    password: akhil  
    chpasswd: { expire: False }  
    ssh_pwauth: True  
    EOF  
                         
  c) cloud-localds user-data.img user-data  
                         
9) Now run the below command.    
   sudo qemu-system-x86_64 -enable-kvm -hda bionic-server-cloudimg-amd64.img -drive "file=user-data.img,format=raw" -m 512 -curses -nographic   
   Now you will be redirected to the New Terminal of Nested VM. Login by providing the username as ubuntu and password as akhil.  
10) To check the functionality update and install the cpuid package.  
    sudo apt-get update  
    sudo apt-get install cpuid  
                         
11) Testing the functionality for 0x4ffffffc and 0x4ffffffd  
    ![Screenshot 2022-12-05 220230](https://user-images.githubusercontent.com/45283425/205833988-2923ba5a-5cbc-4c7e-aac7-6d7e6a711ef1.png)   
    Now run Sudo dmesg to see the below output.  
    ![Screenshot 2022-12-05 220355](https://user-images.githubusercontent.com/45283425/205834224-50b6d0fd-c56b-433c-b5cf-d8a267605281.png)  
