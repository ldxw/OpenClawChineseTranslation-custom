# OpenClawChineseTranslation-custom
这是一个基于 `ghcr.io/1186258278/openclaw-zh` 的自定义 Docker 方案。
## 包含内容

- 自动跟随上游镜像更新
- GitHub Actions 自动构建
- 多架构支持：`amd64` / `arm64`
- 保留上游 Chromium
- 补充常用依赖：
  - `ffmpeg`
  - `python3`
  - `python3-pip`
  - `gh`
  - `jq`
  - `wget`
  - `unzip`
  - `clawhub`
 
## docker-compose.yml
```yaml
services:
  openclaw:
    image: ghcr.io/ldxw/openclaw-zh-custom:latest
    # image: registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:latest
    container_name: openclaw
    volumes:
      - openclaw-data:/root/.openclaw
    environment:
      - OPENCLAW_GATEWAY_TOKEN=${OPENCLAW_GATEWAY_TOKEN:-}
      - NODE_COMPILE_CACHE=/tmp/openclaw-cache
      - OPENCLAW_NO_RESPAWN=1
      - TZ=Asia/Shanghai
      - CHROME_BIN=/usr/bin/chromium
      - PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium
    shm_size: "1gb"
    restart: unless-stopped
    command: openclaw gateway run --allow-unconfigured
    healthcheck:
      test: ["CMD", "wget", "-q", "--spider", "http://localhost:18789/health", "||", "exit", "0"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s

volumes:
  openclaw-data:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: ./openclaw-data
```
