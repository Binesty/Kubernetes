Change password hosts.ini

Remove packages
````bash
ansible-playbook -i hosts.ini clean.yml --ask-become-pass
````

Install Kubernetes
````bash
ansible-playbook -i hosts.ini kubernetes.yml --ask-become-pass
````

Prune Images
````bash
ansible-playbook -i hosts.ini prune.yml --ask-become-pass
````

Update Nodes
````bash
ansible-playbook -i hosts.ini update-nodes.yml --ask-become-pass
````