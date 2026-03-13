# cli-proxy-api 本机记录

## 当前环境
- 用户: `root`
- 当前二进制路径: `/root/cli-proxy-api`
- 配置文件: `/root/config.yaml`
- 日志文件: `/root/cli-proxy-api.log`
- systemd 服务文件: `/etc/systemd/system/cli-proxy-api.service`
- 监听端口: `9527`
- 上游仓库: `https://github.com/router-for-me/CLIProxyAPI`
- 当前已知最新 release: `v6.8.52`

## 当前服务启动方式
```ini
WorkingDirectory=/root
ExecStart=/root/cli-proxy-api --config /root/config.yaml
```

## SSH 真·一键升级口令（最终版）
```bash
VER=$(gh api repos/router-for-me/CLIProxyAPI/releases/latest --jq .tag_name | sed 's/^v//') && \
cd /root && \
curl -fL "https://github.com/router-for-me/CLIProxyAPI/releases/download/v${VER}/CLIProxyAPI_${VER}_linux_amd64.tar.gz" -o /tmp/CLIProxyAPI_linux_amd64.tar.gz && \
tar -xzf /tmp/CLIProxyAPI_linux_amd64.tar.gz -C /tmp && \
cp -a /root/cli-proxy-api "/root/cli-proxy-api.bak_$(date -u +%Y%m%dT%H%M%SZ)" && \
install -m 0755 /tmp/cli-proxy-api /root/cli-proxy-api && \
systemctl daemon-reload && \
systemctl restart cli-proxy-api && \
echo '--- cli-proxy-api version/release ---' && \
echo "release=v${VER}" && \
echo '--- cli-proxy-api status ---' && \
systemctl status cli-proxy-api --no-pager -l | sed -n '1,20p' && \
echo '--- cli-proxy-api pid/time ---' && \
systemctl show cli-proxy-api -p MainPID -p ExecMainStartTimestamp -p ActiveEnterTimestamp && \
echo '--- port 9527 ---' && \
ss -lntp | grep 9527
```

## 分步版升级流程
```bash
VER=$(gh api repos/router-for-me/CLIProxyAPI/releases/latest --jq .tag_name | sed 's/^v//')
cd /root
curl -fL "https://github.com/router-for-me/CLIProxyAPI/releases/download/v${VER}/CLIProxyAPI_${VER}_linux_amd64.tar.gz" -o /tmp/CLIProxyAPI_linux_amd64.tar.gz
tar -xzf /tmp/CLIProxyAPI_linux_amd64.tar.gz -C /tmp
cp -a /root/cli-proxy-api "/root/cli-proxy-api.bak_$(date -u +%Y%m%dT%H%M%SZ)"
install -m 0755 /tmp/cli-proxy-api /root/cli-proxy-api
systemctl daemon-reload
systemctl restart cli-proxy-api
systemctl status cli-proxy-api --no-pager -l | sed -n '1,20p'
systemctl show cli-proxy-api -p MainPID -p ExecMainStartTimestamp -p ActiveEnterTimestamp
ss -lntp | grep 9527
journalctl -u cli-proxy-api -n 80 --no-pager
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
- 升级口令会先自动备份旧二进制，再替换新版，再重启验证
