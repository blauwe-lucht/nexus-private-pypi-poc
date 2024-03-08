Vagrant.configure("2") do |config|
    config.vm.define "rhel8" do |rhel7|
        rhel7.vm.box = "generic/rhel8"

        rh_user = ENV['RH_USER']
        rh_pass = ENV['RH_PASS']

        # Check if the environment variables are set.
        if rh_user.nil? || rh_user.empty? || rh_pass.nil? || rh_pass.empty?
            raise 'Environment variables RH_USER and RH_PASS must be set.'
        end

        # Provision script to register this VM with the RHEL subscription.
        rhel7.vm.provision "shell" do |shell|
            shell.inline = <<-SHELL
                echo 'Registering the system to Red Hat Subscription Management'
                subscription-manager register --username #{rh_user} --password #{rh_pass} --auto-attach
            SHELL
        end

        # Use the trigger feature to deregister when destroying the VM.
        rhel7.trigger.before :destroy do |trigger|
            trigger.name = "Deregistering RHEL"
            trigger.run = { inline: "vagrant ssh -c 'sudo subscription-manager unregister'" }
        end
    end
end
