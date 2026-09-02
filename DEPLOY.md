# edcloudwasm-optimized 部署说明

这是 `edcloudwasm` 的保守优化副本，保留 VLESS/Trojan/SS/SOCKS5/HTTP、WebSocket、xHTTP、WASM 和订阅功能。

## 本次优化

- DoH 查询增加 5 秒超时，避免 DNS 请求长期挂起
- 增加 DNS 正缓存 120 秒、失败缓存 5 秒，最多 256 条
- DoH 查询参数进行 URL 编码
- colo 探测结果增加 10 分钟 TTL，失败后 30 秒重试，避免失败请求每次重复探测
- 错误页请求复用 Promise，减少扫描器/无效路径造成的外部请求
- 错误页返回 404、`no-store`
- 移除 `wrangler.toml` 中的明文 UUID、密码和认证信息，统一使用 Worker Secrets

## 设置 Secrets

```bash
wrangler secret put UUID
wrangler secret put PASSWORD
wrangler secret put SSPASS
wrangler secret put S5HTTPUSER
wrangler secret put S5HTTPPASS
```

不要把真实密钥写回 `wrangler.toml`，也不要提交到 Git。

## 部署

```bash
wrangler deploy
```
