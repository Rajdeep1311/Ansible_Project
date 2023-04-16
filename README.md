# AUTOMATION USING ANSIBLE PLAYBOOKS

![image](https://user-images.githubusercontent.com/113296626/228025105-8b03c9f0-3549-47af-9a30-1d381aae6300.png)

# The Pledge
Created 3-4 EC2 t2.micro instances using the Amazon Machine Image as Ubuntu, designated one of them as the host/master server and the others are termed as nodes. The host server will eventually rollout the updates and changes to the nodes using Ansible which is a Infrastructure as a Code tool. After the instances are up and running, Terraform needs to be installed on the host server. The following commands are used to install Terraform :

```sh
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
```
```sh
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

```
```sh
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint

```

```sh
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

```
```sh
sudo apt update
```
```sh
sudo apt-get install terraform
```

<h3>Note : The following commands may differ according to the AMI used to create the instance(s)</h3>


The following command is used to export the key-pair login file to the host server :
```sh
  scp -i <key-pair.pem> key-pair.pem ubuntu@ec2-43-207-209-136.ap-northeast-1.compute.amazonaws.com:/home/ubuntu/.ssh
 ```

We need to include the host servers' IP Addresses and the key that was imported earlier in the hosts file using the following command :
```sh
  sudo nano /etc/ansible/hosts
 ```
 After doing the following tasks the file should like this 
 
 ![2023-04-16 (3)](https://user-images.githubusercontent.com/113296626/232283671-fb990a2c-e37b-4e8f-bc60-503538f1397a.png)

And give the hosts file read access using :
```sh
  chmod 600 <key-pair.pem>
 ```

# The Turn 

So, the stage is set for the act but we should perform a final check, just to be sure. Using the following command :
```sh
  ansible servers -m ping
 ```
 All the servers should return a "pong" to your request.
 
 Now, before creating the playbooks, we can perform some actions using the module (-m) and the (-a) tag such as :
 ```sh
  ansible servers -a "sudo apt update"
 ```
 Accordingly this commands rolls all the available updates in all the node servers. You can try using other modules from the given link :
 
 https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html
 
 
