# cc-devops-test

devops 测试工程，用于快速部署 k8s 的测试环境

### 如何使用

- 安装依赖 `yarn`
- 一键执行 `yarn test`

### pr 自动上线流程

##### devops 工程

- devops 工程引用 dispatch.yml 的 github action 文件
- devops 工程需要 kubeconfig 和登录跳板机所用的私钥，跳板机的 host 和 port 以明文形式放在 dispatch.yml 文件中

##### 待上线的 pr 工程

- pr 工程引用 ci-backend.yml、ci-frontend.yml 和 pr-build.yml 的 github action 文件
- 以阿里云镜像仓库为例，pr 工程需要 ali_registry、ali_username、ali_password，此外 pr-build.yml 中的 org_name 和 repo_name 需要和阿里云镜像地址一致
- pr 工程还需要提供 personal access token，用于执行 github 的 repository_dispatch 事件，目前采用 cc 的`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`，仅拥有 repo 相关权限
