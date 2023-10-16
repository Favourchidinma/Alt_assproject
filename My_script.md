#Project Task: Deployment of Vagrant Ubuntu Cluster with LAMP Stack

#Objective:

#Develop a bash script to orchestrate the automated deployment of two Vagrant-based Ubuntu systems,
#designated as 'Master' and 'Slave', with an integrated LAMP stack on both systems.

this project features 2 bash scripts:
  a. vagrant.sh
  b. command.sh

vagrant.sh contains scripts that will automate the deployment of the 2 Vagrant-based Ubuntu systems named 'Master' and 'Slave.

command.sh contains scripts that will integrate LAMP Stack on both systems (master and slave). It also contains commands for configuration of the master node as requested in the project.  


#Master node specifications:
  master.vm.hostname = "master"
  master.vm.box = "ubuntu/focal64"
  master.vm.network "private_network", ip: "192.168.20.20"

  config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = "1"

#Slave node specifications:
  slave.vm.hostname = "slave"
  slave.vm.box = "ubuntu/focal64"
  slave.vm.network "private_network", ip: "192.168.20.21"

  config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = "1"
   

#Master node configuration:
(these are contained in the commands.sh script)
  Create a user named altschool.

  Grant altschool user root (superuser) privileges.

  Inter-node Communication:
  Enable SSH key-based authentication:
  The Master node (altschool user) should seamlessly SSH into the Slave node without requiring a password.

  Copy the contents of /mnt/altschool directory from the Master node to /mnt/altschool/slave on the Slave node. This operation should be performed using the altschool user from the Master node.

  Process Monitoring:
  The Master node should display an overview of the Linux process management, showcasing currently running processes.

  Explaining commands for the master node configuration:

  sudo useradd -m -G sudo altschool
  #this adds a user called altschool

  ![alt text](./ALT_assprog/Screenshot%202023-10-16%20174401.png)

  echo -e "favie\nfavie\n" | sudo passwd altschool 

  sudo useradd -ou 0 -g 0 altschool
   #this grant altschool user root (superuser) privileges.

  sudo -u altschool ssh-keygen -t rsa -b 4096 -f /home/altschool/.ssh/id_rsa -N "" -y
   #this creates an SSH key for the altschool user to seamlessly SSH into the Slave node without requiring a password. 

  sudo cp /home/altschool/.ssh/id_rsa.pub altschoolkey

  sudo ssh-keygen -t rsa -b 4096 -f /home/vagrant/.ssh/id_rsa -N ""
  sudo cat /home/vagrant/.ssh/id_rsa.pub | sshpass -p "vagrant" ssh -o StrictHostKeyChecking=no vagrant@192.168.20.21 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
  sudo cat ~/altschoolkey | sshpass -p "vagrant" ssh vagrant@192.168.20.21 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
  sshpass -p "favie" sudo -u altschool mkdir -p /mnt/altschool/slave
   #this creates a recursive directory /mnt/altschool/slave

  sshpass -p "favie" sudo -u altschool scp -r /mnt/* vagrant@192.168.20.21:/home/vagrant/mnt
   #this copies the content of /mnt/altschool/slave to the slave node.

  sudo ps aux > /home/vagrant/running_processes
   #this displays an overview of the Linux process management, showcasing currently running processes, in a directory called running_processes in the /home/vagrant

   ![alt text](./ALT_assprog/Screenshot%202023-10-16%20175435.png)


   LAMP stack (master & slave installation):
    
     (linux, apache, mysql & php):
        - Installing Apache2 Web server
        - Installing PHP & Requirements
        - Installing MySQL
        - Enabling Modules
        - Restarting Apache


lastly a LAMP stack will be installed on both master and slave once the scrip runs. apache is going to be restarted after installation and configuration. 

also mysql server and client are going to be installed on both master and slave

different versions of php is going to be installed and permissions for /var/www is going to be set to an owner and group of "www-data:www-data"



   
