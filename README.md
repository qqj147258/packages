# packages
packages for OpenWRT or LEDE

## 使用方法

假定你已知晓如何编译 OpenWrt 或 LEDE 的固件

1. 编辑`feeds.conf`文件，加入下面一行

```
src-git antigfw https://github.com/anti-gfw/packages.git
```

2. 运行下面的命令

```bash
./scripts/feeds update -a
./scripts/feeds install -a
```

### 预编译固件

* Netgear

    1. [WNDR4300](https://dl.ibrother.me/lede/targets/ar71xx/nand/)

新需求或报错，欢迎提交PR或[Issues](https://github.com/anti-gfw/packages/issues/new)
