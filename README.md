# ansible_docs

This is my dump for Ansible stuff. First observation: Wow, Ansible just works better with my brain :)

## Getting started

First you'll need to choose an installation method that would be most compatible/sustainable with whichever host OS you choose to install Ansible on.  For me, this is currently Debian/Ubuntu, and I have chosen to use pip for my initial installation, which was quite easily done (I've now installed via both pip and apt-get and both appear to work fine, the only difference being that the apt-get method installs to /usr/bin/ansible, and pip installs it to /usr/local/bin/ansible...). Just to get a general feel for how things work, I'd recommend starting off by just issuing some ad hoc commands in Ansible, which unlike Puppet, is *much* easier to do right out of the gate - since you're only using ssh keys, and not dealing with an entire certificate chain.  It's generally a good idea to start off by copying `/etc/ansible/ansible.cfg` to a subdirectory you create under your home directory, i.e. `~/ansible_test`, etc.  This way you'll have an unaltered original for reference later.

Ansible is also relatively easy to configure and run as a non-root user, provided the user you're running it as has the ability to run sudo, and wouldn't be otherwise restricted from gaining local system privileges or need to authenticate through a proxy, etc. (of which there are also relatively straightforward ways by which to handle this condition, i.e. use of the "environment" built-in element in your playbooks, etc...).

## Basic User Management
Since it's likely that you will encounter the need to manage system users and groups as an early task with Ansible, and since I have not encountered much straightforward discussion of this task among the various guides/resources I've consulted initially (most seem more specifically interested in installing/maintaining packages and dependencies...) I'ts perhaps worthwhile to start our discussion and command/config examples specific to to this task. At the moment, I'm still considering what feels like the most scalable foundation for approaching this common task and iterating upon as requirements would seem to change (along with perhaps including instructions and sample code corresponding to a particular use case, i.e. "creating and adding users to a specific group, then installing pyenv with a couple of specific python version for all users of that system group, etc...).  

To be cont'd...



## Links
- This is awesome: https://serversforhackers.com/c/an-ansible-tutorial - no more MCO wrangling for me :).
- The basics of user management: https://dzone.com/articles/user-management-with-ansible
