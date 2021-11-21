---
title: kubernetes 通过ci部署的回滚方式
date: 2021-11-21 22:20:51
tags: kubernetes,circleci,ci,rollout,deploy,rollout status
---

在处理kubernetes的ci自动滚动更新部署时，会使用到命令：
```
kubectl apply -f deployment.yaml
```

但执行完这条命令后，ci脚本会立即结束，因为kubernetes不会等待部署完成才返回结果。

所有以下脚本可以在执行完滚动更新后，等待执行结果，并在失败时进行回滚，放弃掉这次更新。
```
kubectl apply -f myapp.yaml
if ! kubectl rollout status deployment myapp; then
    kubectl rollout undo deployment myapp
    kubectl rollout status deployment myapp
    exit 1
fi
```

> kubectl rollout status deployment myapp  如果部署失败，此命令会以非零返回码退出，以指示失败。