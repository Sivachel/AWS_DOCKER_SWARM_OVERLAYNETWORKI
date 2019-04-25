ansible-playbook -i hosts db_machine.yml
ansible-playbook -i hosts db_provisioning.yml -K
ansible-playbook -i hosts app_machine.yml
ansible-playbook -i hosts app_provisioning.yml -K
