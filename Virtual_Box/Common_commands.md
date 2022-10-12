# Virtual Box 常用命令

[Virtual Box ](https://blog.csdn.net/achang21/article/details/18413811)

[vbox常用命令](https://docs.oracle.com/en/virtualization/virtualbox/6.0/user/vboxmanage-list.html)

# vboxmanage 命令的使用
## 查询所有已经创建的vboxmanage 虚拟机
`vboxmanage list vms`
## 查看所有虚拟机详情
`VBoxManage list vms --long`
## 查看所有正在运行的虚拟机
`VBoxManage list runningvms`
## 关闭虚拟机
`VBoxManage controlvm <vm_name> acpipowerbutton`
## 强制关闭虚拟机
`VBoxManage controlvm <vm_name> poweroff`
## 删除虚拟机
`VBoxManage unregistervm   --delete <vm_name>`
## 查看本地网络
` vboxmanage list  bridgedifs`
