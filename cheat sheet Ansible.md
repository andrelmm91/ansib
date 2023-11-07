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

## modules

<!-- file -->

ansible TARGET -m setup | more
ansible all -m file -a 'path=/tmp/test state=touch' (creating the file)
ansible all -m file -a 'path=/tmp/test state=file mode=600' (changing the permission to 600)
ls -altrh /tmp/test (listening for permission)

<!-- copy -->

ansible all -m fetch -a 'src=/tmp/test_module.txt dest=/tmp/'
ansible all -m copy -a 'src=/tmp/x dest=/tmp/x' (copy to a dest)
ansible all -m copy -a 'remote_src=yes src=/tmp/x dest=/tmp/y' (copy on the remote target using a remote source to a remote dest)

<!-- command -->

ansible all -a "hostname" -o
ansible all -a 'touch DIR/FILE creates=DIR/FILE'
ansible all -a 'rm DIR/FILE removes=DIR/FILE'

 <!-- documentation -->

ansible-doc MODULE

## playbooks

ansible-playbook FILE.YAML
time ansible-playbook FILE.YAML (how much time it takes)

ansible centos1 -m setup -a "gather_subset=network" | more
ansible centos1 -m setup -a "filter=ansible_mem\*" | more
