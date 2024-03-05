<!--
 * @Description: 
 * @Author: mali
 * @Date: 2024-01-22 17:38:01
 * @LastEditTime: 2024-01-22 17:41:07
 * @LastEditors: VSCode
 * @Reference: 
-->
# 安装k8s Dashboard
应用推荐配置，这里需要注意修改v2.6.0为兼容上面安装的kubelet的版本，具体可查看https://github.com/kubernetes/dashboard/releases
> kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.0/aio/deploy/recommended.yaml

# 创建用户并获取token
## 创建admin-user
> kubectl apply -f dashboard-adminuser.yaml
## 绑定cluster-admin授权
> kubectl apply -f dashboard-clusteradmin.yaml
# 获取登录token
开启代理并且设置代理端口为8001
kubectl proxy --port=8001
打开新的命令窗口，执行
检查用户信息是否存在

> curl 'http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/serviceaccounts/admin-user'

获取token不带参数
> curl 'http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/serviceaccounts/admin-user/token' -H "Content-Type:application/json" -X POST -d '{}'

获取token带参数
> curl 'http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/serviceaccounts/admin-user/token' -H "Content-Type:application/json" -X POST -d '{"kind":"TokenRequest","apiVersion":"authentication.k8s.io/v1","metadata":{"name":"admin-user","namespace":"kubernetes-dashboard"},"spec":{"audiences":["https://kubernetes.default.svc.cluster.local"],"expirationSeconds":7600}}'

# 登录k8s Dashboard
复制上一步返回的token信息，浏览器访问如下地址，填入token即可登录
> http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
