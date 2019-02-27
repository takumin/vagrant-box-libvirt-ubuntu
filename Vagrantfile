Vagrant.require_version '>= 2.2.3'
Vagrant.configure('2') do |config|
  config.vagrant.plugins = 'vagrant-libvirt'
  config.vm.box = 'ubuntu1804'
  config.vm.box_url = 'file://./ubuntu-amd64-bionic-virtual.box'
  config.vm.provider :libvirt do |libvirt|
    libvirt.graphics_type = 'spice'
    libvirt.graphics_ip = '0.0.0.0'
    libvirt.graphics_port = 5995
    libvirt.video_type = 'qxl'
  end
end
