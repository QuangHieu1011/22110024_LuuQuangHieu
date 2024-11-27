# Lab #2,22110024, Lưu Quang Hiếu, INSE330380E_03FIE
# Task 1: Firewall configuration 
**Question 1**:
Setup a set of vms/containers in a network configuration of 2 subnets (1,2) with a router forwarding traffic between them. Relevant services are also required:
- The router is initially can not route traffic between subnets
- PC0 on subnet 1 serves as a web server on subnet 1
- PC1,PC2 on subnet 2 acts as client workstations on subnet 2

**Answer 1**:
-Step1: Create 2 subnet 1 and subnet 2 by `docker network create --subnet=192.168.1.0/24 subnet1` and `docker network create --subnet=192.168.2.0/24 subnet2` and check by `docker network ls` 

![image](https://github.com/user-attachments/assets/9748993d-b64c-4487-b5d4-8b7e4efa22b7)

-Step2: Create Container in Subnet 1 by `docker run -dit --name pc0 --net subnet1 --ip 192.168.1.10 ubuntu` and then Install nginx as server inside the container:

![image](https://github.com/user-attachments/assets/d0d71f8d-fb9b-4887-ba05-7edcd75b8c8a)

![image](https://github.com/user-attachments/assets/884a3ff3-6e9e-435e-a1fe-1630adcf7962)

![image](https://github.com/user-attachments/assets/6360f701-38e1-4cf3-a886-72478a1e8a7a)

- Step3: Create 2 conteiner in Subnet 2 by `docker run -dit --name pc1 --net subnet2 --ip 192.168.2.11 ubuntu` and `docker run -dit --name pc2 --net subnet2 --ip 192.168.2.12 ubuntu` then Install the necessary tools inside them (curl, ping) to test the connection by `docker exec -it pc1 bash`and `apt update` and `apt install -y curl iputils-ping`
  
![image](https://github.com/user-attachments/assets/6839e8a0-a560-491e-9177-6a0aa96d32e2)

-Step 4: Crate Router and Check List Network 
![image](https://github.com/user-attachments/assets/17e08207-f307-44f3-aa2d-fb2128874664)

![image](https://github.com/user-attachments/assets/2533d77a-69b6-41f9-83c4-d321b21e211f)

-Step5: Use `docker-compose up -d` to run the set up

![image](https://github.com/user-attachments/assets/69eddf11-2a0f-42bc-91e7-68346ad60893)
**Question 2**:
- Enable packet forwarding on the router.
- Deface the webserver's home page with ssh connection on PC1

**Answer 2**:
* Enable packet forwarding on the router:

  ![image](https://github.com/user-attachments/assets/c86d0934-218e-401a-8946-75a7ef581fc7)
* Deface the webserver's home page with ssh connection on PC1
  
  - Step1: Install SSH in PC1
    
  ![image](https://github.com/user-attachments/assets/9ab7fe11-acd1-4635-ad94-086467d6121c)
 
  - Step2 : Install Web Server on PC0 and Apache running on PC0

  ![image](https://github.com/user-attachments/assets/090a6665-cdfd-4515-b00a-00e70028b89d)

  -Strp 3: Edit the ssh file with `nano /etc/ssh/sshd_config` and change the PermitRootLogin yes and PasswordAuthentication yes
  
  ![image](https://github.com/user-attachments/assets/bd6b44a6-092b-4dec-888c-6dc18266979e)

  Then connect SSH from pc1 to pc0:

  ![image](https://github.com/user-attachments/assets/1acccafe-b42d-43a9-801f-f8fcedd516a6)
  
  Once connected successfully, you can replace the Apache web home page by editing index.html by `echo "<html><body><h1>Hacked by PC1</h1></body></html>" > /var/www/html/index.html` and then Confirm that you have successfully changed the home page by accessing the website from PC1 by `curl http://192.168.1.10`
  ![image](https://github.com/user-attachments/assets/54b8a92a-c92d-4c27-b9d4-730a91d5c8be)
  
  **Question 3**:
  Config the router to block ssh to web server from PC1, leaving ssh/web access normally for all other hosts from subnet 1.
  **Answer 3**:
  
  ![image](https://github.com/user-attachments/assets/449c320c-190b-4ee5-bd94-7408690f1bb6)

  **Question 4**:
- PC1 now servers as a UDP server, make sure that it can reply UDP ping from other hosts on both subnets.
- Config personal firewall on PC1 to block UDP accesses from PC2 while leaving UDP access from the server intact.
  **Answer 4**:
  - Step1 : Create UDP on PC1 by `nc -lu 12345` to Listen UDP on Port 12345
  ![image](https://github.com/user-attachments/assets/c49efb8b-50df-4488-ad38-9c65614fa3e7)
  -Step 2: Configure Firewall to Block UDP from PC2 by `iptables -A INPUT -p udp --dport 12345 -j ACCEPT` and `iptables -A INPUT -p udp --dport 12345 -s 192.168.1.20 -j DROP`
 ![image](https://github.com/user-attachments/assets/921e955f-543b-42a8-a3ff-983951692a37)

# Task 2: Encrypting large message 
Use PC0 and PC2 for this lab 
Create a text file at least 56 bytes on PC2 this file will be sent encrypted to PC0
**Question 1**:
Encrypt the file with aes-cipher in CTR and OFB modes. How do you evaluate both cipher in terms of error propagation and adjacent plaintext blocks are concerned. 
**Answer 1**:
* Demonstrate your ability to send file to PC0 to with message authentication measure.
  - Step 1 : Create  file on PC2
  ![image](https://github.com/user-attachments/assets/9e3d504b-327a-4f51-aac6-03ae56e75463)
  - Step2: Encrypt files with AES with `openssl enc -aes-256-ctr -in file.txt -out file_ctr.enc -pass pass:yourpassword`
  ![image](https://github.com/user-attachments/assets/4b9b20b3-baf4-4927-979d-ee91a51e0cfa)


- Verify the received file for each cipher modes












