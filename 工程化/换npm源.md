---
title: 换npm源
tags: 工程化
categories:
  - 学习呦
date: 2025-10-14 21:01:35
---

## 淘宝源

```Shell
# 命令

npm config set registry https://registry.npmmirror.com

# 验证命令

npm config get registry

# 如果返回https://registry.npmmirror.com，说明镜像配置成功
```

## 腾讯

```Shell
# 命令

npm config set registry http://mirrors.cloud.tencent.com/npm/

# 验证命令

npm config get registry

# 如果返回http://mirrors.cloud.tencent.com/npm/，说明镜像配置成功
```



