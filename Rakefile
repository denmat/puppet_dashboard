  desc 'disable SELINUX'
  task :disable_selinux do
    %x(sed -i 's/SELINUX=enforcing/SELINUX=permissive/' /etc/sysconfig/selinux)
    %x(setenforce 0)
  end

  desc 'set_iptables'
  task :set_iptables, :port do |x, args|
    %x(lokkit -q -p #{args[:port]})
  end

  desc 'this install puppet repo'
  task :get_puppet_repo do
    %x(yum install -y http://yum.puppetlabs.com/el/6/products/x86_64/puppetlabs-release-6-5.noarch.rpm)
    %x(yum install -y http://mirror.iprimus.com.au/epel/6/i386/epel-release-6-7.noarch.rpm)
  end

  desc 'install puppet client binaries'
  task :get_puppet_client => :get_puppet_repo do
    %x(yum makecache)
    %x(yum update -y)
    %x(yum install -y puppet )
  end

  desc 'install and configure '
  task :install_puppet_dashboard do 
    Rake::Task['disable_selinux'].invoke
    Rake::Task['set_iptables'].invoke('8080:tcp')
    Rake::Task['set_iptables'].invoke('3000:tcp')
    Rake::Task['get_puppet_client'].invoke
  end

task :default => :install_puppet_dashboard