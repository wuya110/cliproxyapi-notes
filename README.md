# cli-proxy-api 本机记录

## 当前环境
- 用户: `root`
- 二进制路径: `/root/cli-proxy-api`
- 配置文件: `/root/config.yaml`
- 日志文件: `/root/cli-proxy-api.log`
- systemd 服务文件: `/etc/systemd/system/cli-proxy-api.service`
- 监听端口: `9527`

## 当前服务启动方式
```ini
WorkingDirectory=/root
ExecStart=/root/cli-proxy-api --config /root/config.yaml
```

## SSH 下一次稳妥升级口令（模板）
> 把下面的 `下载/替换二进制` 那一段替换成你实际的获取方式。

```bash
# 1) 下载或替换新二进制到 /root/cli-proxy-api
# 2) 然后执行：
systemctl daemon-reload && \
systemctl restart cli-proxy-api && \
echo '--- cli-proxy-api status ---' && \
systemctl status cli-proxy-api --no-pager -l | sed -n '1,20p' && \
echo '--- cli-proxy-api pid/time ---' && \
systemctl show cli-proxy-api -p MainPID -p ExecMainStartTimestamp -p ActiveEnterTimestamp && \
echo '--- port 9527 ---' && \
ss -lntp | grep 9527
```

## 当前一键重启+验证口令
```bash
systemctl daemon-reload && \
systemctl restart cli-proxy-api && \
echo '--- cli-proxy-api status ---' && \
systemctl status cli-proxy-api --no-pager -l | sed -n '1,20p' && \
echo '--- cli-proxy-api pid/time ---' && \
systemctl show cli-proxy-api -p MainPID -p ExecMainStartTimestamp -p ActiveEnterTimestamp && \
echo '--- port 9527 ---' && \
ss -lntp | grep 9527
```

## 常用排查命令
```bash
systemctl status cli-proxy-api --no-pager -l
journalctl -u cli-proxy-api -n 100 --no-pager
tail -n 100 /root/cli-proxy-api.log
ss -lntp | grep 9527
```

## 备注
- 2026-03-13 起，`cli-proxy-api` 已正式收编到 systemd
- 不再建议手工 `nohup` 或直接在 shell 里裸跑
- 以后统一用 `systemctl restart cli-proxy-api` 管理
