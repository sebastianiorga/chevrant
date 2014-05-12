VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.omnibus.chef_version = "11.4.0"

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.network :forwarded_port, host: 3000, guest: 3000
  config.vm.network :forwarded_port, host: 3306, guest: 3306

  config.vm.synced_folder "rails_app/", "/home/vagrant/rails_app"

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "apt"
    chef.add_recipe "build-essential"
    chef.add_recipe "rvm::vagrant"
    chef.add_recipe "rvm::system"
    chef.add_recipe "git"
    chef.add_recipe 'nodejs'

    chef.add_recipe "postgresql::server"
    chef.add_recipe "mysql::server"


    chef.json.merge!({
      :postgresql => {
                    password: {
                      postgres: 'pass'
                    }
                },
      :rvm => {
        :default_ruby => 'ruby-2.1.0',
        global_gems: [{
            'name'    => 'bundler',
            'version' => '1.5.1'
        }]
      },
      :mysql => {
        "server_root_password" => "root",
        "server_repl_password" => "root",
        "server_debian_password" => "root",
        "allow_remote_root" => true
      }
    })
  end
end
