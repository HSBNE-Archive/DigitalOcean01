* bin script for running commands.
  ansible --limit=development commands/makeadmin.yml --private-key=~/.vagrant.d/insecure_private_key

* detect when we're running commands against vagrant and push through the right extra args with bin/ansible
