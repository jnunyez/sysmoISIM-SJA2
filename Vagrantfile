vendor  = ENV["VENDOR"] || 0x0bda
product = ENV["PRODUCT"] || 0x0169
model = ENV["CONTROLLER"] || "nec-xhci"

Vagrant.configure("2") do |config|
    config.vm.define "sim-programmer"
    config.vm.box = "fedora/34-cloud-base"
    config.vm.provider :libvirt do |libvirt|
      libvirt.cpus = 2
      libvirt.memory = 2048
      #class 0x0b vendor  0x0bda product 0x0169
      libvirt.usb_controller :model => model
      libvirt.usb :vendor => vendor, :product => product
    end
    config.vm.provision :shell, path: "bootstrap"
end
