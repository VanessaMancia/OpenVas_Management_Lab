### In this lab we will create a vulnerability management server using OpenVAS and a virtual machine (VM) and install some insecure software. We will then do scans and do some remediation.

<details close>

#### First thing we will do is create our free Azure account and go to the Azure portal. 

![New Note](https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/04b42b08-5302-45ad-9ebe-e2f02aebcc57)

---
#### Now we will prepare our vulnerability management scanner, which will be used to scan our vulnerable VM.

#### Go to the search bar and type in "marketplace" once we are there type "OpenVas" and click on the one that is supported by HOSSTED.

<img width="617" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/2de094b4-ca7f-4bf9-847c-7c32522cd9a7">

---

#### Once we click on "start with pre set configuration" we will pick the weakest one as shown below.

<img width="337" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/9c379f25-88e0-4ca3-bec8-2cf19a275c82">

---

#### For the VM we are creating we want to name our resource group "Vulnerability-Management" and the VM name "OpenVAS." 

<img width="816" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/c0044f34-e3f6-4df4-be58-a140839c0a5c">

#### For authentication purposes we want to click on "password" and make a username and password that you will remember. 

<img width="824" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/66b9d424-c63d-4768-81bf-b720e3005e68">


#### Go to "monitoring" and disable boot diagnostics. Now click on "review and create" and make sure everything looks good. 

<img width="512" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/713fd3e0-decc-4645-bbc5-64940ebec702">

---

#### After the VM has been created, SSH into the OpenVAS VM we created with PowerShell (windows) or Terminal (MacOS) using the credentials you created earlier. 

#### Quick explanation: SSH (secure shell) is used to connect and manage Linux machines over the internet

#### As shown below, we got the public IP address of our OpenVAS VM and typed it in our terminal and managed to login. 


<img width="718" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/2f74d05f-f724-452d-8fa2-90dadc789361">

#### It should show the web app URL and default username and password at this point, attempt to go to the URL in the browser and login with the username and password. If it doesn’t work, try admin/admin:

<img width="715" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/fc4fa8d2-01ba-4994-9d9d-4b25eba61919">

---

#### After you get logged in, reset the admin password from the original, to: "enter your password" 

<img width="1136" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/97ccbe54-248c-4b59-930d-deb7ace33aeb">

----

<details close>

### We will now create a client VM and make it vulnerable 

#### Go to your azure portal and search for "virtual machine" and click "create" 

<img width="817" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/5d4e6aaa-7686-4fd7-9204-8c2bdc559024">

---

#### After the VM has been created, ensure you can RDP into it with the credentials you created. If you are using a Mac go to the app store and download "Microsoft Remote Desktop" 

#### Go back to Azure and copy the public IP address of the windows-vm and paste it to your RDP.

<img width="809" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/977f4361-6023-4d96-9038-39f46c2ffeb8">

---

#### We are now going to disable the windows firewall. Type in wf.msc

<img width="895" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/a5dd9f31-816b-4d36-8103-1fbd6935a49b">

Go to "window defender firewall properties" and click on "off" for each category then hit "okay" and "apply." 

<img width="895" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/85fcc337-22df-4f97-8065-cdd1c539caeb">


---

#### Let's download and install some old software.

<img width="1249" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/02e3673c-b4cd-4c96-8588-ba5ff74dbd16">

#### Now we will restart our VM and leave it alone for a bit. 

---

<details close>


### We will configure and openVAS to perform first unauthenticated scan against our vulnerable VM. 

#### Unauthenticated means that the vulnerability management platform won't attempt to log into the computer and really look in depth at it. It will scan it from a superficial level from the network. 

---

#### We are going to use our previous link form before to openVAS. Once you are logged in go to Assets → Hosts → New Host


#### Now go to Azure and find your Windows machine and search for the private IP address under networking. 

<img width="419" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/8452811d-6869-4d33-a38a-f40f83ad4add">

#### Go back to openVAS and once you put in the private IP address we will create a new target from the host and name it "Azure Vulnerable VMs" 

---

#### We will create a new task by going to Scans → Tasks → New Tasks

<img width="808" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/29a66dbb-5f32-493b-8638-f245b57b4a80">

#### "Start" the "Scan - Azure Vulnerable VMs" Task. Once done click where it says last report. 


<img width="1435" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/82ad05da-87dd-4929-8768-b1154f179753">

---

#### Once we open the report we can see that none of our super vulnerable stuff is showing. For example, the old software we downloaded and installed into our windows-vm. 


<img width="1435" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/52f5f2ea-96bd-4250-bd4e-1c1f03c9ea83">

---

### We will now make configurations for credentialized scans (within VM) 

#### We are going to RDP into our Windows-VM and disable user account control. 


<img width="835" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/6626b986-4627-4404-a1b0-f60758326378">

---

#### Let's enable remote registry 

### Go to services app and find "remote registry" and set it to automatic. 


<img width="835" alt="image" src="https://github.com/VanessaMancia/OpenVas_Management_Lab/assets/112146207/8ff39c2d-420b-4c65-a783-e3ad082ab7a2">







