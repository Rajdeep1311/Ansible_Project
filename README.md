# AUTOMATION USING ANSIBLE PLAYBOOKS

![image](https://user-images.githubusercontent.com/113296626/228025105-8b03c9f0-3549-47af-9a30-1d381aae6300.png)

# The Pledge
Created 3-4 EC2 t2.micro instances using the Amazon Machine Image as Ubuntu, designated one of them as the host/master server and the others are termed as nodes. The host server will eventually roll out the updates and changes to the nodes using Ansible which is an Infrastructure as a Code tool. After the instances are up and running, Terraform needs to be installed on the host server. The following commands are used to install Terraform :

```sh
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

<h3>Note: The following commands may differ according to the AMI used to create the instance(s)</h3>


The following command is used to export the key-pair login file to the host server :
```sh
 scp -i <key-pair.pem> key-pair.pem ubuntu@ec2-43-207-209-136.ap-northeast-1.compute.amazonaws.com:/home/ubuntu/.ssh
 ```

We need to include the host servers' IP Addresses and the key that was imported earlier in the hosts file using the following command :
```sh
 sudo nano /etc/ansible/hosts
 ```
 After doing the following tasks the file should be like this 
 
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
 Accordingly, this command rolls all the available updates in all the node servers. You can try using other modules from the given link :
 
 https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html
 
 An Ansible playbook is a file containing a series of tasks and configurations to be executed on remote hosts. Here is an example of a basic Ansible playbook:
 
 ```sh
 ---
- name: My playbook
  hosts: my_hosts
  become: true
  vars:
    my_var: "my_value"
  tasks:
    - name: Install packages
      apt:
        name:
          - package1
          - package2
        state: present

    - name: Create directory
      file:
        path: /my/directory
        state: directory
        owner: root
        group: root
        mode: '0755'

    - name: Copy file
      copy:
        src: /path/to/local/file
        dest: /path/to/remote/file
        owner: root
        group: root
        mode: '0644'
```
 Inventories are created namely **development** and **production** to group the remote servers accoordingly and also to deploy certain actions according to the need.
 
 # The Prestige
 
 Now, the final part i.e. rolling out the actions to the required servers.
 
 ```sh
 ansible-playbook <playbook_name>.yaml
 ```
 You can now check the required server instances to check if the playbooks are working properly.
 

 
 
