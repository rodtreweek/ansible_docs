# ansible_docs

This is my dump for Ansible stuff. Maybe I should look into the "Wiki" tab above....

## Getting started

First you'll need to choose an installation method that would be most compatible/sustainable with whichever host OS you choose to install Ansible on.  For me, this is currently Debian/Ubuntu, and I have chosen to use pip for my initial installation, which was quite easily done (I've now installed via both pip and apt-get and both appear to work fine, the only difference being that the apt-get method installs to /usr/bin/ansible, and pip installs it to /usr/local/bin/ansible...). Just to get a general feel for how things work, I'd recommend starting off by just issuing some ad hoc commands in Ansible **on some Vagrant/Docker test systems** (don't do this on any "live" systems...trust me, it's *very* easy to screw something up, and it's pretty easy to just spin something up with Docker, etc. to practice with...), which unlike Puppet, is *much* easier to do right out of the gate - since you're only using ssh keys, and not dealing with an entire certificate chain.  It's generally a good idea to start off by copying `/etc/ansible/ansible.cfg` to a subdirectory you create under your home directory, i.e. `~/ansible_test`, etc.  This way you'll have an unaltered original for reference later.

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

We *all* love to think *"wow, that's pretty stupid...I would never make that mistake..."* - but what none of us *really* realize (right on up to that precise, humbling moment when its our turn to face the music of this particular funeral dirge...) is that sometimes you might not even be able to **see** that critically disruptive error that you are about to make.  

This can *very* easily result from having encountered some weird, inexplicable terminal issue (see below), or some other anomaly appearing seemingly out of nowhere, or perhaps after you do a `sudo bash` or start a new tmux session, rendering command-line input/output *invisibly* or *incorrectly*. For example, you might find yourself squinting initially at a jumbled, barely coherent mess of output from `ctrl + r`, which you might then attempt to "fix" through a combination of `ctrl + arrow/a/e`, backspace, etc., which then leads to the realization that now *your cursor has suddenly disappeared*, before ultimately what you *absolutely thought* was a lengthy yet benign command-line "ssh-in-a-for-loop"-string containing a harmless `ls -ld` *now featuring the terrifying inclusion of an `rm -rf`*, which was somehow "concatenated" from a previous command -- **or** the alteration of the relative scope of an ad hoc `ansible` command - once *only* targeting localhost/vagrant/docker test instances - now unexpectedly matching either something/some logic in your Ansible `hosts` file, ansible.cfg, a file in your`group_vars`/`host_vars` directories, or something else you forgot/didn't anticipate that's been **invisibly truncated/isn't appearing in stdout** like, for example, a trailing `--extra-vars` flag, which you have now just unintentionally executed across your entire infrastructure :( 

<img src="https://raw.githubusercontent.com/rodtreweek/i/master/ansible/term_probs.gif" height="450">

**Bottom line: Think very hard about doing any form of arbitrary "mass ssh" direct command execution across hosts where the scope may not be immediately clear, and I STRONGLY urge you to avoid using the Ansible shell module ad hoc from the command-line unless you absolutely cannot accomplish what you seek in a more structured and suitably contained manner -- AND are absolutely certain you have functional backups for any damage you might do.** 

It's certainly hard to resist the temptation - especially in an emergency situation, or when the pressure of approaching deadlines or "quarterly goals" looms large - but it's almost always going to be the wrong approach.  If you *absolutely believe* that direct command execution is necessary as opposed to properly structuring your roles and playbooks in a manner that would enforce the *desired state* of your infrastructure, give yourself the rest of the day to think about it before you pull the trigger, since you might not *actually see* that the gun is pointed directly at your head :(


## Links
- This is awesome: https://serversforhackers.com/c/an-ansible-tutorial - no more MCO wrangling for me :).
- Some suggestions on user management here: https://dzone.com/articles/user-management-with-ansible
- Ansible Galaxy vid here: https://codereviewvideos.com/course/ansible-tutorial/video/ansible-galaxy-tutorial
- Ansible Galaxy role for pyenv: https://github.com/pyenv/pyenv
- Traffic control: https://blog.scottlowe.org/2015/12/24/running-ansible-through-ssh-bastion-host/
