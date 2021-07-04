# cc-devops-test

devops 测试工程，用于快速部署 k8s 的测试环境

### 如何使用

- 安装依赖 `yarn`
- 一键执行 `yarn test`

### pr 自动上线流程

##### 注意事项

- 执行前，需要将 devops 工程部署到跳板机
- 各个 action 变量
- yarn site:xxx 需要 github 组织名

##### devops 工程

- devops 工程引用 dispatch.yml 的 github action 文件
- devops 工程需要登录跳板机所用的私钥，跳板机的 host 和 port 以明文形式放在 dispatch.yml 文件中
- devops 工程还需要提供 PR_TOKEN(personal access token)，用于执行 github 的 issue_comments api，其中 cc 的`Z2hwXzJCQ1JzSU5zYmNpbXdVVmdQRGJqejhDTzl1VjFaSDJST0pnYQ==`(base64，直接写入 token，github 会直接删除对应的 personal access token)，仅拥有 repo 相关权限

##### 待上线的 pr 工程

- pr 工程引用 ci-backend.yml、ci-frontend.yml 和 pr-build.yml 的 github action 文件
- 以阿里云镜像仓库为例，pr 工程需要 ALI_REGISTRY、ALI_USERNAME、ALI_PASSWORD，此外 pr-build.yml 中的 org_name 和 repo_name 需要和阿里云镜像地址一致
- pr 工程也需要 PR_TOKEN

### adv-uat 卸载方法
```
k -n adv-uat delete deployments.apps finance
k -n adv-uat delete deployments.apps hall
k -n adv-uat delete deployments.apps kanban
k -n adv-uat delete deployments.apps tower
k -n adv-uat delete deployments.apps bank
k -n adv-uat delete deployments.apps club
helm uninstall mongodb -n adv-uat
helm uninstall postgresql -n adv-uat
k -n adv-uat get pvc
k -n adv-uat delete pvc data-postgresql-postgresql-0
kubectl delete ns adv-uat
```
