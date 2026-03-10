# OpenClaw 自定义镜像说明
> 说明：本文档由 GPT 辅助生成并整理，已根据当前 OpenClaw 中文版镜像仓库结构进行适配，使用前请自行核对配置与构建细节。

基于底包：
`ghcr.io/1186258278/openclaw-zh:latest`

构建三档镜像：
- low
- standard
- full

---

## 镜像版本说明

### low 版本
标签示例：

`ghcr.io/ldxw/openclaw-zh-custom:low`

`registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:low`

用途：
- 低配机器
- 1核 / 1G
- 仅基础 OpenClaw 能力
- 适合 QQ Bot / 企业微信基础使用
- 不建议本地跑 Whisper

额外内容：
- ffmpeg
- python3
- python3-pip
- edge-tts

---

### standard 版本
标签示例：

`ghcr.io/ldxw/openclaw-zh-custom:standard`

`ghcr.io/ldxw/openclaw-zh-custom:latest`

`registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:standrd`

`registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:latest`

用途：
- 推荐默认版本
- 2核 / 2G 及以上
- 支持常用语音、中文字体、PDF 技能
- 适合大多数用户

额外内容：
- jq
- qrencode
- ffmpeg
- python3
- python3-pip
- poppler-utils
- libglib2.0-0
- fonts-noto-cjk
- fonts-noto-color-emoji
- fonts-wqy-zenhei
- faster-whisper
- edge-tts

---

### full 版本
标签示例：

`ghcr.io/ldxw/openclaw-zh-custom:full`

`registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:full`

用途：
- 全功能版本
- 4核 / 4G 以上更合适
- 支持本地 Whisper
- 支持 Chromium 无头浏览器
- 最大兼容 SkillHub

额外内容：
- jq
- qrencode
- ffmpeg
- python3
- python3-pip
- poppler-utils
- libglib2.0-0
- Chromium 常见运行库
- 中文字体
- openai-whisper
- faster-whisper
- edge-tts

---

## latest 标签说明
默认：
`latest -> standard`

原因：
- 功能平衡
- 资源占用适中
- 最适合作为默认版本

---

## 工作流说明
工作流文件：
`.github/workflows/docker-build.yml`

功能：
- 每 6 小时检测一次底包是否更新
- 支持手动触发
- 支持强制重构
- 同时构建：low、standard、full
- 推送到：GHCR、阿里云镜像仓库
- 每个版本自动附带日期标签 年.月.日

文件对应关系：
`Dockerfile.low       -> low`

`Dockerfile.standard  -> standard / latest`

`Dockerfile.full      -> full`

---

## 构建结果标签示例

### low
`ghcr.io/ldxw/openclaw-zh-custom:low`

`registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:low`

### standard
`ghcr.io/ldxw/openclaw-zh-custom:standard`

`registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:standrd`

`ghcr.io/ldxw/openclaw-zh-custom:latest`

`registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:latest`

### full
`ghcr.io/ldxw/openclaw-zh-custom:full`

`registry.cn-hangzhou.aliyuncs.com/ldxw/openclaw-zh-custom:full`

---

## 使用示例

### 拉取低配版
`docker pull ghcr.io/ldxw/openclaw-zh-custom:low`

### 拉取标准版
`docker pull ghcr.io/ldxw/openclaw-zh-custom:standard`

### 拉取全功能版
`docker pull ghcr.io/ldxw/openclaw-zh-custom:full`

### 拉取默认最新版
`docker pull ghcr.io/ldxw/openclaw-zh-custom:latest`

---

## 运行示例
`docker run -d \
  --name openclaw \
  -p 18789:18789 \
  -v ./openclaw-data:/root/.openclaw \
  ghcr.io/ldxw/openclaw-zh-custom:standard`

---

## 注意事项
- `/root/.openclaw` 为数据持久化目录。
- 底包已经自带健康检查，派生镜像无需重复写入。
- low 版本更适合低配机器，建议语音识别走 API。
- full 版本更适合本地 STT、浏览器自动化和完整技能生态。
