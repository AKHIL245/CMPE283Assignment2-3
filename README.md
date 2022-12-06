
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
