# Virtualization
虚拟化学习笔记
## 批量创建虚拟机
> vagrant 可以批量创建虚拟机，支持 Windows linux mac系统

[根据操作系统下载](https://www.vagrantup.com/downloads)

我这里以windows系统为例
[vagrant_2.3.0_windows_amd64.msi](https://releases.hashicorp.com/vagrant/2.3.0/vagrant_2.3.0_windows_amd64.msi)

下载完成后 点击安装 vagrant_2.3.0_windows_amd64.msi 注意：不能有中文的安装路径否则会报错

我使用的虚拟机为 [Oracle VM VirtualBox](https://download.virtualbox.org/virtualbox/6.1.36/VirtualBox-6.1.36-152435-Win.exe)

# vagrant 批量创建的脚本
创建一个名称为 Vagrantfile 的文件并写入下面脚本信息

```Vagrantfile
Vagrant.configure("2") do | config|
	(1..3).each do |i|
		config.vm.define "k8s-node#{i}" do |node|
			# 设置虚拟机的Box
			node.vm.box = "centos/7"

			# 设置虚拟机的主机名
			node.vm.hostname="k8s-node#{i}"

			# 设置虚拟机的IP
			node.vm.network "private_network", ip: "192.168.56.#{99+i}", netmask: "255.255.255.0"

			# 设置主机与虚拟机的共享目录
			# node.vm.synced_folder "~/Documents/vagrant/share", "/home/vagrant/share"

			# VirtaulBox相关配置
			node.vm.provider "virtualbox" do |v|
				# 设置虚拟机的名称
				v.name = "k8s-node#{i}"
				# 设置虚拟机的内存大小
				v.memory= 4096
				# 设置虚拟机的CPU个数
				v.cpus = 4
			end
		end
	end
end
```
在 Vagrantfile 文件路径下打开cmd终端并执行

` vagrant up ` 就会自动下载虚拟机镜像并自动创建

在创建过程中打开 VirtualBox 可看见正在创建的过程

待创建完成可在cmd中使用 `vagrant ssh k8s-node1` 进入虚拟机环境中

就此虚拟机环境创建完成

如果忘记配置可执行 `vagrant ssh-config` 查看每个虚拟机的配置信息

如修改vagrantfile文件后可执行 `vagrant reload` 加载更新配置

## 关闭虚拟机

`vagrant halt` # 关闭所有虚拟机

`vagrant halt  virtual name` # 指定虚拟机名称进行关闭
