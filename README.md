# arm-toolchain-for-embedded-bin

AUR 包 [`arm-toolchain-for-embedded-bin`](https://aur.archlinux.org/packages/arm-toolchain-for-embedded-bin) 的 GitHub 镜像仓库。

package 本体：上游 [arm/arm-toolchain](https://github.com/arm/arm-toolchain) 的 **ATfE**
（Arm Toolchain for Embedded，LLVM 裸机工具链），安装到 `/opt/atfe`，
避免与官方 `clang`/`llvm`/`lld` 包的文件冲突。

## 自动更新

`.github/workflows/update.yml` 每周一自动检查上游 ATfE release：

1. 取最新的 `release-X.Y.Z-ATfE` 标签，并下载官方 `.sha256` 校验和
2. 更新 `PKGBUILD` 的 `pkgver` / `sha256sums_*`（`pkgrel` 重置为 1）
3. 在 `archlinux:base-devel` 容器里完整构建验证并生成 `.SRCINFO`
4. 提交到本仓库，同时推送到 AUR

手动触发：Actions → *Update AUR package* → *Run workflow*（可指定版本，留空用最新）。
上游资产改名或缺失时任务会失败并发送邮件通知。

## 需要的 Secret

| 名称 | 说明 |
| --- | --- |
| `AUR_SSH_KEY` | CI 部署私钥，对应公钥已登记在 AUR 账号的 SSH Key 列表里 |

## 本地手动更新

```bash
# 修改 PKGBUILD 里的 pkgver 与 sha256sums_*
makepkg --printsrcinfo > .SRCINFO
git commit -am "upgpkg: arm-toolchain-for-embedded-bin <ver>-1" && git push
```

## 备注

- 上游 ATfL（Arm Toolchain for Linux）目前没有可下载的二进制 release，因此未打包
- newlib / newlib-nano 为实验性 overlay，也未打包
