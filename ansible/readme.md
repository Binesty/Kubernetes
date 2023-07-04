Change password hosts.ini

Remove packages
````bash
ansible-playbook -i hosts.ini clean.yml --ask-become-pass
````

Install Kubernetes
````bash
ansible-playbook -i hosts.ini kubernetes.yml --ask-become-pass
````