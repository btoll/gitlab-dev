# -*- mode: ruby -*-
# vi: set ft=ruby :
#

Vagrant.configure("2") do |config|
    config.vm.box =  "debian/bullseye64"
    config.vm.hostname = "gitlab-dev"
    config.ssh.forward_agent = true
    config.vm.synced_folder ".", "/vagrant"

    config.vm.provision "shell", inline: <<-SHELL
        apt-get update && \
        apt-get install -y \
            git

        locale-gen en_US.UTF-8
        localectl set-locale LANG=en_US.UTF-8
    SHELL

    config.vm.provision "ansible_local" do |ansible|
        ansible.become = true
        ansible.groups = {
            "development" => ["default"]
        }

        ansible.compatibility_mode = "2.0"
        ansible.playbook = "playbook.yml"
        ansible.version = "latest"
        ansible.extra_vars = {
            ansible_python_interpreter: "/usr/bin/python3"
        }

        ansible.galaxy_role_file = "requirements.yml"
        ansible.galaxy_roles_path = "/etc/ansible/roles"
        ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
    end
end

