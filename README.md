# ansible_docs

This is my dump for Ansible stuff. First observation: Wow, Ansible just works better with my brain :)

## Getting started

First you'll need to choose an installation method that would be most compatible/sustainable with whichever host OS you choose to install Ansible on.  For me, this is currently Debian/Ubuntu, and I have chosen to use pip for my initial installation, which was quite easily done (I've now installed via both pip and apt-get and both appear to work fine, the only difference being that the apt-get method installs to /usr/bin/ansible, and pip installs it to /usr/local/bin/ansible...). Just to get a general feel for how things work, I'd recommend starting off by just issuing some ad hoc commands in Ansible, which unlike Puppet, is *much* easier to do right out of the gate - since you're only using ssh keys, and not dealing with an entire certificate chain.  It's generally a good idea to start off by copying `/etc/ansible/ansible.cfg` to a subdirectory you create under your home directory, i.e. `~/ansible_test`, etc.  This way you'll have an unaltered original for reference later.

Ansible is also relatively easy to configure and run as a non-root user, provided the user you're running it as has the ability to run sudo, and wouldn't be otherwise restricted from gaining local system privileges or need to authenticate through a proxy, etc. (of which there are also relatively straightforward ways by which to handle this condition, i.e. use of the "environment" built-in element in your playbooks, etc...).

Since it's likely that you will encounter this as an early task, it's worthwhile to discuss how you might undertake very basic user management w/ Ansible - LDAP/more advanced integrations will be discussed (much) later. Once I have a basic/reasonable playbook for this I'll add it to this repo.  Until then, I'll be hunting around for what feels like a more or less scalable foundation for approaching this common task (along with perhaps a corresponding use case description, i.e. "installing pyenv with a couple of specific python version for all users of a particular system group, etc...).  To be cont'd...



## Links
- This is awesome: https://serversforhackers.com/c/an-ansible-tutorial - no more MCO wrangling for me :).
- The basics of user management: https://dzone.com/articles/user-management-with-ansible
