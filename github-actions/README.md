# action 说明

### ci-backend.yml

`用于执行service服务的ci`

### pr-build.yml

`当工程pr被创建、重新打开、同步时，重新build镜像，并推送到阿里云，最后调用repository_dispatch事件调用devops项目进行部署`

### main-build.yml

`主分支main push时，重新build镜像，并推送到阿里云，最后调用repository_dispatch事件调用devops项目进行部署`

### pr-dispatch.yml

`通过repository_dispatch事件触发，ssh至跳板机，部署pr环境`

使用 ssh 到跳板机上，执行相应的 devops 的 ansible 文件，存在如下问题

- 目前跳板机的 git 没有设置登录，如果更新，需要手动拷贝 devops 工程到跳板机，未来考虑将 devops 打包为镜像，并运行在 k8s 上，可能存在如何连接至相应 pod 的问题

### main-dispatch.yml

`通过repository_dispatch事件触发，ssh至跳板机，部署uat环境`

- 存在与 pr-dispatch.yml 相同的问题

### ansible.yml

`用于直接通过ansible-playbook直接上线pr镜像，包括安装python、ansible、kubectl`
在执行 Play Ansible Playbook 存在两个问题
`问题1是由于community.kubernetes新版本造成的，在requirements.txt指定老版本即可`

- ~~提示 'template' is only supported parameter for 'k8s' module~~
- 执行 ansible，若使用 action 模板，则只能使用系统内的 python，若使用 run 执行 ansible-playbook 指令，则 env 变量无法作用于 yml 文件内

### k8s.yml

`用于直接通过kubectl执行镜像pod的yml文件`
目前已弃用该方案
