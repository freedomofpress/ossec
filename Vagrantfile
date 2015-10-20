# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  config.vm.define 'build' do |build|
    # Run check for extra vars file
    check_for_extra_vars_file

    build.vm.box = "build"
    build.vm.box = "trusty64"
    build.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
    build.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/build-deb-pkgs.yml"
      ansible.verbose = 'v'
    end
    build.vm.provider "virtualbox" do |v|
      v.name = "ossec-build"
    end
  end

end


# The vars for OSSEC checksums are fetched via `rake`
# and written to `ansible_vars.json`, so may not exist.
# The vars are absolutely required for running the build playbook,
# however, so if the vars file is missing ,the play will error out,
# and nary a helpful error message. Let's catch the error
# before invoking the play, so folks know what to do.
def check_for_extra_vars_file
  repo_root = File.expand_path(File.dirname(__FILE__))
  ossec_dynamic_vars_file = File.join(repo_root, "ansible_vars.json")
  if not FileTest.file?(ossec_dynamic_vars_file)
    puts "Failed to find ansible_vars.json file."
    puts "Run `bundle install`, then `bundle exec rake`."
    puts "See the README for details."
    exit(1)
  end
end
