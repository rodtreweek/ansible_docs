# Ansible_docs

This is my dump for Ansible stuff. Maybe I should look into the "Wiki" tab above....

## Getting started

First you'll need to choose an installation method that would be most compatible/sustainable with whichever host OS you choose to install Ansible on.  For me, this is currently Debian/Ubuntu, and I have chosen to use pip for my initial installation, ~which was quite easily done (I've now installed via both pip and apt-get and both appear to work fine, the only difference being that the apt-get method installs to /usr/bin/ansible, and pip installs it to /usr/local/bin/ansible...)~ **Update:** After some *extreme* sluggishness using pip to install/update ansible on WSL, and the perplexing disappearance of my config file location after upgrading to 2.4.2, I've decided to switch back to using `apt-get` to install/manage the ansible package. Here's what I did:
```
$ sudo -EH pip uninstall ansible
$ sudo -E apt-get update
$ sudo -E apt-get install software-properties-common
$ sudo -E apt-add-repository ppa:ansible/ansible
$ sudo -E apt-get update
$ sudo -E apt-get install ansible
```

Just to get a general feel for how things work, I'd recommend starting off by just issuing some ad hoc commands in Ansible **on some Vagrant/Docker test systems** (don't do this on any "live" systems...trust me, it's *very* easy to screw something up, and it's pretty easy to just spin something up with Docker, etc. to practice with...), which unlike Puppet, is *much* easier to do right out of the gate - since you're only using ssh keys, and not dealing with an entire certificate chain.  It's generally a good idea to start off by copying `/etc/ansible/ansible.cfg` to a subdirectory you create under your home directory, i.e. `~/ansible_test`, etc.  This way you'll have an unaltered original for reference later.

Once you ar finished with the installation you can run `ansible --version` - which will give you a decent amount of useful info such as the `config file`, `configured module search path`, and relative locations for the ansible executable, ansible python module, and current python version use to run ansible.

**Note:** If you are trying to get ansible working from within WSL, you may encounter this error:
`ERROR! Unexpected Exception, this is probably a bug: 'module' object has no attribute 'SSL_ST_INIT'`

If you run into this error, do the following to fix it:
```
$ sudo -E apt-get remove python-cryptography
```
```
$ sudo -E apt-get install python-setuptools
```

Ansible is also relatively easy to configure and run as a non-root user, provided the user you're running it as has the ability to run sudo, and wouldn't be otherwise restricted from gaining local system privileges or need to authenticate through a proxy, etc. (of which there are also relatively straightforward ways by which to handle this condition, i.e. use of the "environment" built-in element in your playbooks, etc...).

Next, once you get comfortable with the basics, the super-important concept of "roles' will start to make sense.  I'd suggest doing a fair bit with the ad hoc commands and getting familiar with some of the underlying constructs available for use in your yaml/playbooks first, but you should *defintely* spend some time understanding roles immediately following (once you understand roles, then you will be pretty floored by what's available via `ansible-galaxy` - which would seemingly surpass what's available through Chef, Salt, Puppet, etc. - (I'll however give a nod to Puppet-DB here as to what's available relative to more massive scale-out considerations..)

## Basic User Management
Since it's likely that you will encounter the need to manage system users and groups as an early task with Ansible, and since I have not encountered much straightforward discussion of this task among the various guides/resources I've consulted initially (most seem more specifically interested in installing/maintaining packages and dependencies...) It's perhaps worthwhile to start our discussion and command/config examples here. At the moment, I'm still getting a feel for what seems like the most scalable foundation for approaching this task, and iterating on it as requirements would seem to change (along with including instructions and sample code corresponding to a particular use case, i.e. "creating and adding users to a specific group, then installing pyenv with a couple of specific python version for all users of that system group, etc...).

To be cont'd...

## **Update and MASSIVE WORD OF CAUTION**

Ok. I'll just come clean here and say that Ansible *will allow you to fuck things up in powerfully destructive ways that Puppet will absolutely and very intentionally prevent you from doing, unless you opt to use some **completely unsupported and risky** "foreign language github MCO shell module" - the existence of which you only discovered after a solid 45 minutes of determined googling - that allows you to do a direct exec across multiple hosts ...*

Soooooo - all this to say: "Stay tuned for my next github installment where I discuss being *completely accountable and sincerely apologizing to all the people whose files you just obliterated*, while - and this is important - *resisting the urge to defend yourself*".

### Immediately followed by Part II in the series:

"Rapidly getting to work figuring out how to use `testdisk` and `photorec` along with some clever shell/python scripts you've hacked together for sorting through (hopefully) all the cryptically-named, root-owned files you now (again, hopefully) have saved on a separate partition you've thoughfully created - with ample space - smartly anticipating just such an occassion where you may need to recover stuff you likely won't have any backups for".

### Which should then naturally include the FAQ item:

1. How to answer the question: "Why wasn't any of this backed-up?" - which should really just link back to the first "apology" article referenced above.

**I don't think I can state this more emphatically, *sometimes the problems with doing some form of arbitrary/ad hoc "mass exec"-style command on the command-line aren't immediately obvious***

We *all* love to think *"wow, that's pretty stupid...I would never make that mistake..."* - but what none of us *really* realize - right on up to that precisely humbling moment when its our turn to face the music of this particular funeral dirge - is that sometimes **we might not even be able to see the critically disruptive error that we are about to make** - due to reasons ranging from severe terminal emulation glitches -

<img src="https://raw.githubusercontent.com/rodtreweek/i/master/ansible/term_probs.gif" height="450">

- to "sh*t...did I accidentally put my wife's contact lenses in by mistake today??"*, or a combination of both personal and technical failings which then lead to squinting initially at a jumbled, barely coherent mess of output from `ctrl + r`, which we might then attempt to "fix" through a combination of `ctrl + arrow/a/e`, backspace, etc. - then leading to the sudden realization that now our *cursor has suddenly disappeared* - before ultimately what we *absolutely thought* was a lengthy, yet benign command-line "ssh-in-a-for-loop"-string containing a harmless `ls -ld` instead having somehow *reached backward into our history buffer to retrieve and concatenate a destructive `rm -rf` invisibly to the end of our ad-hoc ansible command* - once *only* targeting localhost/vagrant/docker test instances - now unexpectedly matching either something/some logic in our Ansible `hosts` file, ansible.cfg, a file in `group_vars`/`host_vars` directories, or something else we forgot/didn't anticipate which can't be observed in STDOUT :(

**Bottom line: Think very hard about doing any form of arbitrary "mass ssh" direct command execution across hosts where the scope/implications may not be immediately clear, and I STRONGLY urge you to avoid using the Ansible shell module ad hoc from the command-line unless you absolutely cannot accomplish what you seek in a more structured and suitably contained manner -- AND are absolutely certain you have functional backups for any damage you might do.**

It's certainly hard to resist the temptation - especially in an emergency situation, or when the pressure of approaching deadlines or "quarterly goals" looms large - ~but it's almost always going to be the wrong approach~ but it's infrequently necessary to do so under anything other than some pretty unique circumstances - which should almost certainly lead to immediate root cause analysis, and nomination as candidates for additional process-engineering.  If you *absolutely believe* that direct command execution is necessary as opposed to properly structuring your roles and playbooks in a manner that would enforce the *desired state* of your infrastructure, give yourself the rest of the day to think about it before you pull the trigger, since *you might not actually see* that the gun is pointed directly at your head :(

## Some General Troubleshooting Notes...

Ok, so this came up, and I was initially scratching my head as to how this could possibly be happening...

``
ansible-playbook <inventory file> -K
SUDO password:
{…}
PLAY [server-name1] ************************************************************

TASK [Gathering Facts] *********************************************************
fatal: [server-name1]: FAILED! => {"msg": "Incorrect sudo password"}
ok: [server-name2]
{…}
PLAY RECAP *********************************************************************
server-name1                     : ok=17   changed=1    unreachable=0    failed=0
server-name2                     : ok=0    changed=0    unreachable=0    failed=1
```
What the ??...

So I log into server-name1, just to see if I can indeed sudo with the *same exact password*:
```
 my_user@server-name1 $ sudo su -                                   
[sudo] password for my_user:
Last login: Tue Jan {…}
[root@server-name1~]# exit
logout
Time: 10
```

Yep, just as i suspected, it's fine. - So, I try running my ansible-playbook command exactly as before:
```
TASK [Gathering Facts] *********************************************************
ok: [server-name1]
ok: [server-name2]
{…}
PLAY RECAP *********************************************************************
server-name1                    : ok=17   changed=0    unreachable=0    failed=0
server-name2                  : ok=17   changed=1    unreachable=0    failed=0

...And this time it's fine.  I of course changed nothing...

 Ok,  so after a pretty protracted troubleshooting effort making my way through each layer right on up through "Application", I found several things that were problematic...
 * First, I had forgotten that I had changed the IP address on my “control” host (my laptop).
 * Followed by the discovery that the `/etc/sudoers` files were different on the above two hosts
 * I also had an error in my Ansible `ssh.cfg` file where I had duplicated my control_path entry (with different values)
 
 Here's the result of running my playbook again *without* the -K option - *which should fail on both hosts* if /etc/sudoers isn't actually configured to allow password-less access to the wheel group (which, since these were my own personal dev/test hosts configured for “learning mode”, I’d opted for convenience over security and previously added an entry granting the wheel group root-level privileges…).
```
ansible-playbook <inventory file>
{…}
TASK [Gathering Facts] **********************************************************************
fatal: []: FAILED! => {"changed": false, "module_stderr": "Shared connection to server-name1 closed.\r\n", "module_stdout": "sudo: a password is required\r\n", "msg": "MODULE FAILURE", "rc": 1}
ok: [server-name2]
```

Ok, so it hits me:
"Dude, remember when you f’d up and had to re-install sudo on server-name1? Yep...when you tried to install Vim 8 on that host, you accidentally removed "vim-minimal" - which is a sneaky bit of dependency trickery that actually takes along sudo if you choose to remove it without carefully reviewing the dependency list prior...”
…ok so here is the missing "warning label" that everyone should get along with (at least) CentOS 7.x:

*WARNING!!!!  if you remove vim-minimal, you may also end-up removing sudo, and by extension the /etc/sudoers file - which will likely then screw up a lot of stuff like the use of ansible for login and execution of privileged tasks.*

Again, in my case since these were personal test/dev systems I’d set up for practice, and was allowing password-less wheel on one host – but was using the default /etc/sudoers file on the other (due to the reinstallation of sudo on that host) - and thus not allowing wheel access this was why I needed to enter a password on server-name1 - post-screwup while installing Vim 8.

All this to say I guess, that for whatever it’s worth, if your ssh.cfg doesn't contain duplicate control paths *and* you’ve added your user to the wheel group, while `%wheel  ALL=(ALL)   NOPASSWD: ALL` exists in your /etc/sudoers file on each of the remote hosts you intend to manage, *and* no other gid/uid/restrictive path entries would appear to be holding you back - you shouldn't actually need to use the -K option when running ansible-playbook commands -- however, under most circumstances this is probably not a great idea, and unless you have a compelling reason not to, you should really require the use of -K when issuing commands with `sudo` on remote hosts :)


## Links
- This is awesome: https://serversforhackers.com/c/an-ansible2-tutorial - no more MCO wrangling for me :).
- Some suggestions on user management here: https://dzone.com/articles/user-management-with-ansible
- Ansible Galaxy vid here: https://codereviewvideos.com/course/ansible-tutorial/video/ansible-galaxy-tutorial
- Ansible Galaxy role for pyenv: https://github.com/pyenv/pyenv
- Traffic control: https://blog.scottlowe.org/2015/12/24/running-ansible-through-ssh-bastion-host/
- Best Practices: http://docs.ansible.com/ansible/latest/playbooks_best_practices.html
- Ansible administration from within WSL (Windows Subsystem for Linux): https://www.jeffgeerling.com/blog/2017/using-ansible-through-windows-10s-subsystem-linux and: https://www.jeffgeerling.com/blog/running-ansible-within-windows
- Galaxy cheatsheet: http://cheat.readthedocs.io/en/latest/ansible/galaxy.html
