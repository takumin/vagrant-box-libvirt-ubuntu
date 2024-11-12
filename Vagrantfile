# vim: set ft=ruby :
ENV['MIRROR_UBUNTU_ARCHIVE'] ||= 'http://archive.ubuntu.com/ubuntu'
ENV['MIRROR_UBUNTU_SECURITY'] ||= 'http://security.ubuntu.com/ubuntu'
Vagrant.require_version '>= 2.4.1'
Vagrant.configure('2') do |config|
  config.vagrant.plugins = ['vagrant-libvirt']
  config.vm.box = 'ubuntu2404'
  config.vm.box_url = 'file://./ubuntu-amd64-noble-libvirt.box'
  config.vm.provider :libvirt do |libvirt|
    libvirt.random :model => 'random'
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
cat > /etc/apt/sources.list.d/ubuntu.sources << __EOF__
Types: deb
URIs: #{ENV['MIRROR_UBUNTU_ARCHIVE']}
Suites: ${DISTRIB_CODENAME} ${DISTRIB_CODENAME}-updates ${DISTRIB_CODENAME}-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: #{ENV['MIRROR_UBUNTU_SECURITY']}
Suites: ${DISTRIB_CODENAME}-security
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
__EOF__
# Apt Update
apt-get -y update
apt-get -y dist-upgrade
|
end
