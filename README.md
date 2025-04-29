SSH is used for authentication. No agent is required

Generate pair key in the main host

ssh-keygen

We can copy to all hosts aimed for automation:

for user in ansible root; 
    do   
        for os in ubuntu centos;   
        do    
             for instance in 1 2 3;     
                do       
                    sshpass -f password.txt ssh-copy-id -o StrictHostKeyChecking=no ${user}@${os}${instance};     
                done;   
        done; 
done

Test connectivity

ansible -i,ubuntu1,ubuntu2,ubuntu3,centos1,centos2,centos3 all -m ping

git clone https://github.com/spurin/diveintoansible.git

#Ansible configuration

ansible --version
    config file priority
        4) /etc/ansible/ansible.cfg
        3) ~/.ansible.cfg
        2) ./ansible.cfg
        1) ANSIBLE_CONFIG env variable

#Ansible Inventories

ansible all -m ping

--> ansible file 

[defaults]
inventory = host

--> host file

[all]
centos1

ANSIBLE_HOST_KEY_CHECKING=False ansible all -m ping

All hosts are add to all '*' group by default

[defaults]
inventory = hosts
host_key_checking = False ****

ansible <group> --list-hosts

-o can be used for oneline

ansible all -m ping -o
ansible ~.*3 --list-hosts

~ = regex
. = any character
* = any number of times

ansible all -m command -a 'ls' -o

module -m command the default module wth ansible

ansible_become ==> become superuser
ansible_port=2222 ==> change port for SSH //// centos:2222 ansible_user=root

[control]
ubuntu-c ansible_connection=local

[centos]
centos1 ansible_port=2222
centos[2:3]

[centos:vars]
ansible_user=root

[ubuntu]
ubuntu[1:3]

[ubuntu:vars]
ansible_become=true
ansible_become_pass=password

[linux:children]
centos
ubuntu

#Setup module
ansible centos -m setup | more

#File module
ansible all -m file -a 'path=/tmp/test state=file mode=600'
ls -altrh /tmp/test
ansible all -m file -a 'path=/tmp/test state=touch'

#Copy module
ansible all -m copy -a 'src=/tmp/x dest=/tmp/x'
ansible all -m copy -a 'remote_src=yes src=/tmp/x dest=/tmp/y'

#Fetch module
ansible all -m fetch -a 'src=/tmp/test_modules.txt dest=/tmp/'

#### Documentation ####

ansible-doc <module>

<<<< YAML >>>>

Can optionally start with --- and end with ...

python3 -c 'import yaml,pprint;pprint.pprint(yaml.load(open("test.yaml").read(), Loader=yaml.FullLoader))'