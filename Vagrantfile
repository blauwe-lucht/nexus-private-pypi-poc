Vagrant.configure("2") do |config|
    # Ansible control server:
    config.vm.define "acs" do |acs|
        acs.vm.box = "generic/alma8"
        acs.vm.hostname = "acs"

        acs.vm.provision "shell" do |shell|
            shell.inline = <<-SHELL
                set -euo pipefail

                yum install -y epel-release

                echo "Installing Ansible..."
                yum install -y ansible
                # NOTE: this will break once ansible will install a newer version of python3.
                python3.11 -m ensurepip
                # jmespath is needed by role ansible-ThoTeam.nexus3-oss:
                python3.11 -m pip install jmespath
                su -l vagrant -c "ansible-galaxy role install geerlingguy.java"
                su -l vagrant -c "ansible-galaxy role install ansible-ThoTeam.nexus3-oss"
            SHELL
        end

        # Make sure all sensitive info is only readable by user.
        acs.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
    end

    # Nexus server:
    config.vm.define "nexus" do |nexus|
        nexus.vm.box = "generic/alma8"
        nexus.vm.hostname = "nexus"

        nexus.vm.network "private_network", ip: "192.168.7.33"
   end

   # Internet-less server:
    config.vm.define "nointernet" do |nointernet|
        nointernet.vm.box = "generic/alma8"
        nointernet.vm.hostname = "nointernet"

        nointernet.vm.network "private_network", ip: "192.168.7.34"

        # Disable internet. Doing this with Ansible is harder, so we do it like this:
        nointernet.vm.provision "shell" do |shell|
            shell.inline = <<-SHELL
                set -euxo pipefail

                yum update -y
                
                yum install -y python3

                yum install -y firewalld
                systemctl enable --now firewalld
                
                # Allow HTTP(S) to 192.168 subnet:
                firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 0 -p tcp --dport 80 -d 192.168.0.0/16 -j ACCEPT
                firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 0 -p tcp --dport 443 -d 192.168.0.0/16 -j ACCEPT

                # Block HTTP(S) to everything else:
                firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp --dport 80 -j REJECT
                firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp --dport 443 -j REJECT

                firewall-cmd --reload
                firewall-cmd --list-all
            SHELL
        end
   end
end
