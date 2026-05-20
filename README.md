# 封锁 BT / 挖矿 / 测速 流量 (block-bt-mining-speedtest)

`fscs.sh` —— 一键封锁 BitTorrent、挖矿矿池、测速网站相关流量的管理脚本（含自愈守护）。

## 用途

通过 iptables / ip6tables 的字符串匹配（`-m string`）在 OUTPUT 链丢弃含特定关键字的流量，常用于禁止机器跑 BT 下载、连接矿池或被滥用跑测速。规则通过 systemd 服务持久化，并可启用看门狗自动恢复。

## 屏蔽关键字（节选）

- **BT**：`torrent`、`info_hash`、`announce`、`BitTorrent protocol`、`magnet:`、迅雷 `xunlei` / `Thunder` 等
- **挖矿**：`ethermine.com`、`antpool.com`、`pool.bar` 等矿池域名
- **测速**：`speedtest.net/.cn`、`fast.com`、`speed.cloudflare.com`、`test.ustc.edu.cn` 等

## 使用方法

```bash
bash fscs.sh {install|uninstall|reload|watchdog|stopwatch}
```

| 命令 | 说明 |
|---|---|
| `install` | 安装并启用封锁规则（写入 `/usr/local/bin/` + systemd 服务）|
| `uninstall` | 卸载规则与服务 |
| `reload` | 手动重载规则 |
| `watchdog` | 开启自动检测与恢复（每分钟检测规则是否丢失）|
| `stopwatch` | 停止自动检测 |

## 依赖与环境

- 权限：**必须 root**
- 依赖：`iptables`（需 `string` 匹配模块）、`systemd`

## 注意事项

- 基于关键字的字符串匹配可能**误伤**正常流量（如包含相关词的网页/接口），请按需调整脚本内的 `RULES` 列表
- 仅作用于本机出站（OUTPUT）流量
