# vim: set ft=ruby :
ENV['MIRROR_UBUNTU'] ||= 'http://archive.ubuntu.com/ubuntu'
Vagrant.require_version '>= 2.2.19'
Vagrant.configure('2') do |config|
  config.vagrant.plugins = ['vagrant-libvirt']
  config.vm.box = 'ubuntu2204'
  config.vm.box_url = 'file://./ubuntu-amd64-jammy-libvirt.box'
  config.vm.provider :libvirt do |libvirt|
    libvirt.random :model => 'random'
    libvirt.storage_pool_name = 'ramdisk'
    libvirt.graphics_type = 'spice'
    libvirt.graphics_ip = '0.0.0.0'
    libvirt.graphics_port = 5995
    libvirt.video_type = 'qxl'
  end
  if Vagrant.has_plugin?('vagrant-proxyconf')
    config.proxy.no_proxy = "#{ENV['no_proxy'] || ENV['NO_PROXY']}" if ENV['no_proxy'] || ENV['NO_PROXY']
    config.proxy.ftp      = "#{ENV['ftp_proxy'] || ENV['FTP_PROXY']}" if ENV['ftp_proxy'] || ENV['FTP_PROXY']
    config.proxy.http     = "#{ENV['http_proxy'] || ENV['HTTP_PROXY']}" if ENV['http_proxy'] || ENV['HTTP_PROXY']
    config.proxy.https    = "#{ENV['https_proxy'] || ENV['HTTPS_PROXY']}" if ENV['https_proxy'] || ENV['HTTPS_PROXY']
  end
  config.vm.synced_folder '.', '/vagrant', type: 'rsync', rsync__exclude: ['.git/', '*.box', '*.img']
  config.vm.provision 'shell', inline: %Q|
# Get Codename
. /etc/lsb-release
# Apt Repository
cat > /etc/apt/sources.list << __EOF__
deb     #{ENV['MIRROR_UBUNTU']} ${DISTRIB_CODENAME}           main restricted universe multiverse
deb-src #{ENV['MIRROR_UBUNTU']} ${DISTRIB_CODENAME}           main restricted universe multiverse
deb     #{ENV['MIRROR_UBUNTU']} ${DISTRIB_CODENAME}-updates   main restricted universe multiverse
deb-src #{ENV['MIRROR_UBUNTU']} ${DISTRIB_CODENAME}-updates   main restricted universe multiverse
deb     #{ENV['MIRROR_UBUNTU']} ${DISTRIB_CODENAME}-backports main restricted universe multiverse
deb-src #{ENV['MIRROR_UBUNTU']} ${DISTRIB_CODENAME}-backports main restricted universe multiverse
deb     #{ENV['MIRROR_UBUNTU']} ${DISTRIB_CODENAME}-security  main restricted universe multiverse
deb-src #{ENV['MIRROR_UBUNTU']} ${DISTRIB_CODENAME}-security  main restricted universe multiverse
__EOF__
# Apt Update
apt-get -y update
apt-get -y dist-upgrade
|
end
