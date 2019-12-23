---
title: 使用ansible-playbook推送安装nginx1111
date: 2018-10-12 22:57:12
tags: ansible
categories: 自动化运维
---
# 使用ansible-playbook推送安装nginx
> 本文主要介绍了如何将ansible-playbook投入生产中，下面我们以推送安装nginx举例

#### 1. 思路：
- 现在主机上编译，配置好nginx，
- 然后将安装程序目录打包
- 最后用ansible分发下去

#### 2. 流程图如下：
![image](https://s1.ax1x.com/2018/10/11/itW3sf.png)


#### 3. 执行配置过程
##### 3.1 编译安装nginx详见： [编译安装Nginx](http://note.youdao.com/noteshare?id=b73effc8df9b50ad2b63f8f0ed2ba676&sub=0D01183B5BCA44C593B382FB2AE2FE3D)

##### 3.2 创建ansible roles目录结构
```bash

cd /etc/ansible/
mkdir -pv nginx_install/rolse/{common,install}/{handlers,files,meta,tasks,templates,vars} 
```

> 说明：
> - roles下有两个角色，common为一些准备操作，install为安装nginx的操作
> - 每个角色下面又有6个目录，
>   - handlers 下面是当发生改变时，要执行的操作，通常用在配置文件发生改变，重启服务等
>   - flies 为安装时用到的一些文件
>   - meta 为说明信息，说明角色依赖等信息
>   - tasks 里面为核心配置文件
>   - templates 通常存一些配置文件，启动脚本等模板文件
>   - vars 下为定义的变量
 
##### 3.3 将安装好的nginx程序包打包，并放到 `$/install/files/` 下面，启动脚本 放入`$/install/templates`中，
```bash
cp /etc/init.d/nginx /root/nginx_install/rolse/install/templates/nginx
mv nginx.tar.gz /root/nginx_install/rolse/install/files/
cp nginx/conf/nginx.conf /root/nginx_install/rolse/install/templates/
```

##### 3.4 在common目录下定义安装nginx的依赖包任务
```bash
cd common/tasks
vim main.yml

- name: install initaliztion require software
  #yum: name={{ items }} state=installed
  yum: name="pcre-devel,zlib-devel"
  #with_item
  #   - zlib-devel 
  #   - prce-devel
```

##### 3.5 定义变量
```bash
cd install/vars/main.yml

nginx_user: www
nginx_port: 80
nginx_basedir: /usr/local/nginx
```

##### 3.6 定义拷贝操作
```bash
vim  install/tasks/copy.yml

- name: Copy Nginx Software
  copy: src=nginx.tar.gz dest=/tmp/nginx.tar.gz owner=root group=root
- name: Uncommpreesion Nginx Software
  shell: tar -zxf /tmp/nginx.tar.gz -C /usr/local
- name: Copy Nginx Start Scrpit
  template: src=nginx dest=/etc/init.d/nginx owner=root group=root mode=0755
- name: Copy Nginx Config
  template: src=nginx.conf dest={{ nginx_basedir }}/conf/ owner=root group=root mode=0644
  ```
  
##### 3.7 创建安装总线
```bash
vim install/tasks/install.yml 
 
- name: Creat Nginx User 
  user: name={{ nginx_user }} state=present createhome=no shell=/sbin/nologin
- name: Start Nginx Service 
  shell: /etc/init.d/nginx start
- name: Add Boot Start Nginx Service
  shell: chkconfig --level 345 nginx on
- name : Delete Nginx Commpression files
  shell: mv /tmp/nginx.tar.gz /tmp/old.tgz
```

##### 3.8 创建执行总线 
```bash
vim install/tasks/main.yml

- include: copy.yml
- include: install.yml
```

##### 3.9 创建入口配置文件
``` bash
vim install.yml

---
- hosts: nginxlabs
  remote_user: root
  gather_facts: True
  roles:
    - common
    - install
``` 

