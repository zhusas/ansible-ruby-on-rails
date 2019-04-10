## 实现的功能

- 设置机器名，时区及系统语言编码，ulimit参数
- 创建应用运行用户并安装mina 、jumpserver的公钥，免密登录。
- 安装好相关系统组件依赖包
- 替换APT软件源为国内网易镜像
- 安装docker，Elasticsearch、MongoDB、MySQL、Redis、memcached容器自动根据可自定义的变量文件安装
- MySQL字符集为UTF8，默认打开慢查询。
- NGINX自动编译安装，包含nginx-gridfs组件，可以访问MongoDB里面的文件
- 在部署用户家目录下使用rbenv安装好Ruby环境
- 自动建立应用目录并拉取代码，完成编译，elasticsearch建立索引
- 创建数据库用户并授权，导入定义好的数据sql文件
- 自动启动应用，部署完毕即可访问


## 操作

只在Ubuntu 16.04上测试通过，理论上也适用Ubuntu 14.04和Ubuntu 18.04 LTS，这里就不展开，有兴趣的同学可以直接看代码。

**使用条件**

1、操作的机器安装好Ansible，能正常连接到Internet，DNS能正常解释网站，如何安装可以参照官方文档。

[Ansible官方安装文档](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#intro-installation-guide)



2、编辑目录下的hosts文件，把目标服务器的IP填上去，一行一个。



3、对目标服务器配置ssh免密登录,这里以192.168.0.243举栗子：

```bash
root@Ansible:~# ssh-copy-id root@192.168.0.243
root@Ansible:~# ssh-keyscan 192.168.0.243 >> ~/.ssh/known_hosts


开始裸机部署
root@Ansible:~# ansible-playbook -i hosts -vvv main.yml
```



## 关于变量文件group_vars/all

里面有很多自定义的项目，部署过程中大部分的参数都从这里读取，可以根据项目情况自定义。

### 说明一些常用的，每个项目都会不一样的变量

操作系统层面的参数，如timezone、locale、hostname。如果有多台机器，hostname参数可以在hosts文件中定义，每行一个。

MySQL参数可以设置配置文件目录、数据目录、日志目录在主机上的路径。docker镜像可以有5.6和5.7两种，默认为mysql:5.6，root密码已经定义好，以AES256的方式加密。项目的数据库名称、用户和密码一样在这里定义。使用前得自己修改，具体使用参考ansible的文档中ansible-vault命令。

Redis可以定义数据在主机上的目录

MongoDB可以定义版本、配置文件和数据在主机上的目录，数据库的名称。

Elasticsearch可以定义数据、配置文件、日志在主机上的目录，版本，索引的名称。

NGINX可以自定义版本和安装目录，还有编译安装的参数。

应用部分可以定义部署目录，项目的目录结构，部署的代码在github的地址及分支。rails_env也可以在此定义。

以上参数定义好之后，会自动在部署过程过程中生效。




