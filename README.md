# cf-net 部署指南（Claude Code + Wrangler CLI）

## 前置条件

```bash
npm i -g wrangler
wrangler login
```

## 部署流程

### 1. 配置环境变量

```bash
# Cloudflare Account ID — wrangler 自动识别，无需写入 wrangler.toml
export CLOUDFLARE_ACCOUNT_ID="your-account-id"
```

### 2. 创建 KV 命名空间

```bash
# 创建 KV 并获取 id
wrangler kv namespace create "C"

# 将返回的 id 填入 wrangler.toml 的 [[kv_namespaces]] 段
```

### 3. 注入 Secret

```bash
# u 作为 secret 注入，不写入 wrangler.toml
wrangler secret put u
# 按提示输入 UUID 值
```

### 4. 部署

```bash
wrangler deploy
```

### 5. 验证

```bash
wrangler tail  # 实时查看日志
```

## 本地开发

```bash
cp .dev.vars.example .dev.vars  # 填入本地变量
wrangler dev
```

## 脱敏原则

| 项目 | 处理方式 |
|------|----------|
| `account_id` | `CLOUDFLARE_ACCOUNT_ID` 环境变量 |
| `vars.u` | `wrangler secret put` 注入 |
| KV `id` | 部署时通过 `wrangler kv namespace create` 获取 |

不在 `wrangler.toml` 中硬编码任何账户标识、密钥或业务配置值。
