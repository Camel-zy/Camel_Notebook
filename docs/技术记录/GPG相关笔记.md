---
title: GPG相关笔记
---

GPG相关笔记

<!-- more -->

# 生成密钥

```shell
gpg --gen-key	// 用默认参数生成密钥对
gpg --full-generate-key     // 生成密钥对(列出所有可选项)
gpg -a --gen-revoke [用户ID] > gpg_revoke.rev    // 生成撤销证书
```

# 列出密钥

```shell
gpg --list-secret-keys --keyid-format LONG	// 列出密钥 包含keyid
gpg -a --export [用户ID] > gpg_public.pub   // 导出公钥 -a表示-armor,以ASCII码输出(默认以二进制输出)
gpg -a --export-secret-key [用户ID] > gpg_secret.sec   // 导出私钥
gpg --import .key // 导入公钥或私钥
```

# 发布与吊销
```shell
gpg --keyserver pool.sks-keyservers.net --send-keys [用户ID]    // 发布密钥
gpg --import gpg_revoke.rev // 吊销密钥
gpg --keyserver pool.sks-keyservers.net --send-keys [用户ID]    // 更新吊销信息
```

# 删除密钥
```shell
gpg --delete-keys keyid     // 删除公钥
gpg --delete-secret-keys    // 删除私钥
```

# gpg使用
```shell
gpg -e -r [接收者ID] -o [输出文件] [待加密文件] // 加密 -r表示recipient -e表示encrypt
gpg -d -u [用户ID] -o [输出文件] [加密文件]	// 解密 -d表示decrypt
gpg -u [用户ID] -s [待签名文件]   // 签名
gpg -u [用户ID] -r [接收者ID] -se [待签名加密文件]   // 签名加密同时进行
gpg --verify [待验证文件]  // 验证签名
```

# git中使用gpg
```shell
gpg --list-secret-keys --keyid-format=long  // 列出密钥
git config --global user.signingkey [主键ID]   // 设置git使用的gpg密钥  主键ID即为上述命令列出的sec rsa4096/后的内容
git config --global commit.gpgsign true  // 设置git提交时自动签名
```

最后贴两个参考博客

[http://ruanyifeng.com/blog/2013/07/gpg.html](http://ruanyifeng.com/blog/2013/07/gpg.h)

[https://yexun1995.github.io/2020/09/15/GPG/](https://yexun1995.github.io/2020/09/15/GPG/)

[https://blog.ginshio.org/2020/gpg_started_guide/](https://blog.ginshio.org/2020/gpg_started_guide/)