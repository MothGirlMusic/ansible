# ansible
ansible demo for semaphore on proxmox.

1. on proxmox, install an lxc with turnkey semaphore (its downloadable as a template.) as well as a remote machine running debian or ubuntu (this uses the "apt" command, which is debian based... or you could change it to yum. but i'm using debian for my tutorial.)
2. you'll need to create /home/semaphore/.ssh with the mkdir command and chown -R it to semaphore:semaphore because the turnkey distro for some reason forgot or didn't do that!
3. log into semaphore using the IP address and the username admin and whatever you supplied as the root password. make a new demo project or use the default.
4. create a ssh key with ssh-keygen and put it on your git account and as a ssh key in the keystore.
5. add this repo to your playbook repositories. copy pasta the SSH method. and select the SSH key you added.
6. make a new environment. just name it and put {} in it. this demo doesn't use it, but for some reason semaphore requires it.
7. make a new inventory. name it the name of the remote machine. add your ssh key. (also copy the ssh key files and chown it to /home/semaphore/.ssh folder because semaphore is janky like that) select type static and put in this, only changing your remote machine's IP address:
[webservers]
10.0.2.218
[webservers:vars]
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
8. go into the remote server and do ssh-copy-id root@10.0.2.216 again replacing the ip with your ansible server's and then see if you can login from the ansible terminal to the remote using the SSH key over SSH. if you can't, you may need to edit the remote server's sshd config to allow root login or allow password logins for root so you can initially copy over the key and then disable it.
9. create a new template. alias could be "Install Apache" or something. playbook name should be the filename of the playbook, so "web.yaml". select ssh key, inventory, repository, and the environment we created and save.
10. click run and watch it do it's thing. it SHOULD complete just fine, given the remote host is debian based. if not, fork this and change apt to yum and remove the update plays as yum automatically does that when run.


Notes: you can add the update commands all together into one play but i left it like this showing this as absolutely as simply as possible by having everything be completely seperate.

Enjoy!
