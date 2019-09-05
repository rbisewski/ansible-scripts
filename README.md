# Ansible

If you plan on using the python API features of Ansible then obtaining the
latest version via pip is the best method:

```bash
sudo pip3 install ansible
```

In order to access remote machines, Ansible requires an ssh-agent in order to
be useful. Consider adding your SSH identities like so:

```bash
ssh-agent bash
ssh-add ~/.ssh/name_of_key
ssh-add ~/.ssh/name_of_another_key
```

Once Ansible is installed and an ssh-agent setup, edit the Ansible hosts file:

```bash
sudo vim /etc/ansible/hosts
```

Then add whatever IPs or hosts that you plan to run commands on:

```bash
ibiscybernetics.com
```

This will essentially allow use of the ansible all command to run commands
on all of the machines. Ergo, to ping every single machine in the ansible
hosts file:

```bash
ansible all -m ping
```

To run a command on a single machine use the `-a` flag instead and then
specify the hostname. For example to check the current logs on
ibiscybernetics.com, run the following commands:

```bash
ansible ibiscybernetics.com -a "ls -hla /var/log"
```

This will then print out a list of all of the files and their sizes.

To run a command that requires privilege escalation, use the `--become` or
`--become-user USERNAME` and to run `sudo` commands please use the `-K` flag
rather than `-a "sudo name-of-command"` since this is untrusted. For example,
to update the apt package cache use the following:

```bash
ansible ibiscybernetics.com --become -m shell -a "apt update" -K
```

Using the non-interactive flag with apt means it is also possible to upgrade
Debian / Ubuntu packages on a given host:

```bash
ansible ibiscybernetics.com --become -m shell -a "apt -y upgrade" -K
```

### Using Playbooks

In addition to running simple commands on single or all hosts, Ansible also
allows groups of commands or operations to be conducted together, say as part
of a larger set of commands that need to be executed. These are called
Playbooks and they allow for precise customization of how Ansible behaves.

### Common troubleshooting

Ansible relies on python to carry out commands. Some minimalist installs of
Linux only have python3 and no `python` command for backwards compatibility.
This can be resolved by connecting to the host in question and adding a
symlink:

```bash
sudo ln -s /usr/bin/python3 /usr/bin/python
```
