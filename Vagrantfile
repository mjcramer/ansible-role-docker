VAGRANTFILE_API_VERSION = '2'

ssh_key = File.read("/Users/cramer/.ssh/mjcramer-travis-ci.pub")

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "docker-1"
  config.vm.hostname = "docker-1"
  config.vm.synced_folder '.', '/vagrant', disabled: true

  # config.vm.box = "google/gce"
  # config.vm.provider :google do |google, override|
  #   google.google_project_id = "sandbox-1023"
  #   google.google_client_email = "travis-ci@sandbox-1023.iam.gserviceaccount.com"
  #   google.google_json_key_location = "/Users/cramer/.gcp/mjcramer-travis-ci.json"
  #   google.name = config.vm.hostname
  #   google.image_family = 'ubuntu-1804-lts'
  #   google.preemptible = true
  #   google.auto_restart = false
  #   google.on_host_maintenance = "TERMINATE"
  #   google.metadata = {
  #     "sshKeys" => ssh_key
  #   }
  #
  #   override.ssh.username = "mjcramer"
  #   override.ssh.private_key_path = "~/.ssh/mjcramer-travis-ci"
  # end

  # config.vm.box = "centos/7"
  config.vm.box = "ubuntu/bionic64"
  docker_volume = '/tmp/docker-1.vdi'

  # Virtualbox configuration
  config.vm.provider :virtualbox do |virtualbox|
    virtualbox.name = config.vm.hostname
    virtualbox.customize ['modifyvm', :id, '--memory', '2048']
    virtualbox.customize ['modifyvm', :id, '--ioapic', 'on']
    virtualbox.customize ['modifyvm', :id, '--cpus', '1']
    virtualbox.customize ['modifyvm', :id, '--cpuexecutioncap', '50']
    virtualbox.customize ['modifyvm', :id, '--groups', '/docker']

    unless File.exist?(docker_volume)
      virtualbox.customize ['createhd', '--filename', docker_volume, '--size', 100 * 1024, '--format', 'VDI', '--variant', 'Fixed']
      virtualbox.customize ['modifyhd', docker_volume, '--type', 'writethrough']
    end
    virtualbox.customize ['storageattach', :id, '--storagectl', 'SCSI', '--port', 2, '--type', 'hdd', '--medium', docker_volume]
  end

  config.vm.provision :shell do |shell|
    shell.inline = "sudo apt-get update -y ; sudo apt-get install -y python"
  end

  config.vm.provision :ansible do |ansible|
    ansible.compatibility_mode = "auto"
    # Disable default limit to connect to all the machines
    ansible.limit = "all"
    ansible.playbook = "tests/test.yml"
    ansible.host_vars = {
        'docker-1' => {
            :docker_config_storage_device => '/dev/sdc'
        }
    }
    ansible.extra_vars = {
    }
  end
end
