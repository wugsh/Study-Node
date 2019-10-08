# Gitlab-ce 安装

## GitLab介绍
GitLab：是一个基于Git实现的在线代码仓库托管软件，你可以用gitlab自己搭建一个类似于Github一样的系统，一般用于在企业、学校等内部网络搭建git私服。
功能：Gitlab 是一个提供代码托管、提交审核和问题跟踪的代码管理平台。对于软件工程质量管理非常重要。

    版本：GitLab 分为社区版（CE） 和企业版（EE）。
    配置：建议CPU2核，内存2G以上。

## Gitlab的服务构成：
    Nginx：静态web服务器。
    gitlab-shell：用于处理Git命令和修改authorized keys列表。（Ruby）
    gitlab-workhorse: 轻量级的反向代理服务器。（go）
    GitLab Workhorse是一个敏捷的反向代理。它会处理一些大的HTTP请求，比如文件上传、文件下载、Git push/pull和Git包下载。其它请求会反向代理到GitLab Rails应用，即反向代理给后端的unicorn。
    logrotate：日志文件管理工具。
    postgresql：数据库。
    redis：缓存数据库。
    sidekiq：用于在后台执行队列任务（异步执行）。（Ruby）
    unicorn：An HTTP server for Rack applications，GitLab Rails应用是托管在这个服务器上面的。（Ruby Web Server,主要使用Ruby编写）

## GitLab安装yum安装
官方源地址：https://about.gitlab.com/downloads/#centos6

清华大学镜像源：https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce

### 添加yum源

    cat> /etc/yum.repos.d/gitlab-ce.repo<< EOF
    [gitlab-ce]
    name=Gitlab CE Repository
    baseurl=https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/  
    gpgcheck=0
    enabled=1
    EOF

### 安装依赖包

    yum install curl policycoreutils-python openssh-server

### 安装

    yum makecache    #清除缓存
    yum install gitlab-ce

### 更改访问地址

    vim /etc/gitlab/gitlab.rb
    external_url 'http://0.0.0.0'

### 重新加载配置
    [root@gitlab ~]#   gitlab-ctl reconfigure

### 启动Gitlab

    gitlab-ctl start 

### Gitlab常用命令

    gitlab-ctl start    # 启动所有 gitlab 组件；
    gitlab-ctl stop        # 停止所有 gitlab 组件；
    gitlab-ctl restart        # 重启所有 gitlab 组件；
    gitlab-ctl status        # 查看服务状态；
    vim /etc/gitlab/gitlab.rb        # 修改gitlab配置文件；
    gitlab-ctl reconfigure        # 重新编译gitlab的配置；
    gitlab-rake gitlab:check SANITIZE=true --trace    # 检查gitlab；
    gitlab-ctl tail        # 查看日志；
    gitlab-ctl tail nginx/gitlab_access.log








