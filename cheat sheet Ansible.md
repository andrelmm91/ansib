## step 1

<!-- SSH key -->

ssh-keygen
cat known_hosts
cat .ssh/id_rsa.pub

ssh-copy-id ansible@ubuntu1

<!-- SSH pass -->

sudo apt update
sudo apt install sshpass
echo password > password.txt

<!-- setting ssh comm for all ubuntu channels -->

for user in ansible root
do
for os in ubuntu centos
do
for instance in 1 2 3
do
sshpass -f password.txt ssh-copy-id -o StrictHostKeyChecking=no ${user}@${os}${instance}
done
done
done

<!-- checking password for all instances -->

ansible -i,ubuntu1,ubuntu2,ubuntu3,centos1,centos2,centos3 all -m ping

---

## step 2

<!-- step 2 CONFIGURATION FILE from the least to the highest prio-->

<!-- 2.1 creating a config file -->

su -
mkdir /etc/ansible
touch /etc/ansible/ansible.cfg
ansible --version

<!-- 2.2 ansible.cfg in the user hame directory... hidden file-->

cd ~
pwd (for checking)
touch .ansible.cfg
ansible --version

<!-- 2.3 current directory -->

cd ~
mkdir (folder)
cd (folder)
touch ansible.cfg
ansible --version

<!-- 2.4 environment variable -->

cd ~
touch FILE.cfg
export ANSIBLE_CONFIG=/home/ansible/FILE.cfg
ansible --version

## Inventories

ansible all -m ping -o
ansible all -m command -a 'id' -o

ansible all --list-hosts
ansible GROUP --list-hosts
ansible ~.\*3 --list-hosts (any number any character and ending in 3)

ansible all -m command -a 'id' -o
ansible all -m ping -e 'ansible_port=xx' -o
