# 使用vagrant 批量创建kubernetes集群

[vagrant & kubernetes](https://medium.com/@raj10x/multi-node-kubernetes-cluster-with-vagrant-virtualbox-and-kubeadm-9d3eaac28b98)

---

# Virtualization
虚拟化学习笔记
## 批量创建虚拟机
> vagrant 可以批量创建虚拟机，支持 Windows linux mac系统

[根据操作系统下载vagrant](https://www.vagrantup.com/downloads)

我这里以windows系统为例
[vagrant_2.3.0_windows_amd64.msi](https://releases.hashicorp.com/vagrant/2.3.0/vagrant_2.3.0_windows_amd64.msi)

下载完成后 点击安装 vagrant_2.3.0_windows_amd64.msi 注意：不能有中文的安装路径否则会报错

我使用的虚拟机为 [Oracle VM VirtualBox](https://download.virtualbox.org/virtualbox/6.1.36/VirtualBox-6.1.36-152435-Win.exe)


# Vagrant 使用手册

## vagrant 官网
[vagrant](https://www.vagrantup.com/) 

## 镜像列表

[Vagrant cloud](https://app.vagrantup.com/boxes/search)

## Vagrant basic cmd (vagrant 基础命令的使用)
```bash
# 创建虚拟机
vagrant up
# 查看虚拟机状态
vagrant status
# 查看本地虚拟机
vagrant box list
# 下载虚拟机镜像可选择不同虚拟机平台的系统镜像文件
vagrant box add generic/ubuntu2004
# 指定需要下载的虚拟机平台和系统镜像版本
vagrant box add generic/ubuntu2004 --provider=virtualbox --box-version=3.1.22
# 使用ssh进入虚拟机中
vagrant ssh <host name>
# 查看虚拟机配置
vagrant ssh-config 
or
vagrant ssh-config <host name>
# 暂停虚拟机
# 暂停
vagrant suspend <host name>
# 重新开始
vagrant resume <host name>
# 重新加载
vagrant reload <host name>
# 关机
vagrant halt <host name>

# 删除虚拟机
vagrant destroy <host name>
# 查看全部虚拟机的状态
vagrant global-status
# 清除无效的虚拟机缓存条目
vagrant global-status --prune

# 指定平台和版本号进行删除本地镜像文件
vagrant box remove --provider=hyperv --box-version=3.1.22
# 退出虚拟机
logout
``` 
---
# vagrant 批量创建的脚本

创建一个名称为 Vagrantfile 的文件并写入下面脚本信息

## 写法一：

```ruby
# 使用host_list 批量定义不同的主机名称与虚拟机操作系统
host_list = [

    {
        :name => "host1",
        :box => "centos/7"
    
    },
    {
        :name => "host2",
        :box => "centos/7"
    },
    {
        :name => "host3",
        :box => "centos/8"
    }

]

# 循环创建虚拟机主机
Vagrant.configure("2") do | config|
    host_list.each do | item|
         config.vm.define item[:name] do|host|
            # 设置虚拟机的Box操作系统    
            host.vm.box = item[:box]            
         end
    end
 end

```
## 写法二：

```ruby
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
