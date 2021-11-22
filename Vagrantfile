WORKERS = 3
Vagrant.configure("2") do |config|

  config.hostmanager.enabled           = true
  config.hostmanager.manage_guest      = false
  config.hostmanager.manage_host       = true

  (1..WORKERS).each do |i|
    config.vm.define "worker-#{i}" do |worker|
      worker.vm.box = "bento/ubuntu-20.04"
      worker.vm.hostname = "worker-#{i}"
      # config.vm.network "forwarded_port", guest: 2, host: 7000, host_ip: "127.0.0.1", auto_correct: true
      # worker.vm.provision :hostmanager
      if i == 3
        worker.vm.provision "ansible" do |ansible|
          ansible.force_remote_user = true
          ansible.galaxy_role_file = "requirements.yml"
          ansible.playbook="install.yaml"
          ansible.groups = { 
            "microk8s_group_HA" => ["worker-[1:3]"],
          }
        end
      end
    end
  end 
end
