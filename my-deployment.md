## Docker 部署

**镜像说明：**

TrendRadar 提供两个独立的 Docker 镜像，可根据需求选择部署：

| 镜像名称                     | 用途      | 说明                   |
|--------------------------|---------|----------------------|
| `wantcat/trendradar`     | 新闻推送服务  | 定时抓取新闻、推送通知（必选）      |
| `wantcat/trendradar-mcp` | AI 分析服务 | MCP 协议支持、AI 对话分析（可选） |

> 💡 **建议**：
> - 只需要推送功能：仅部署 `wantcat/trendradar` 镜像
> - 需要 AI 分析功能：同时部署两个镜像

使用 docker compose

1. **创建项目目录和配置**:

   ```bash
   # 克隆项目到本地
   git clone https://github.com/sansan0/TrendRadar.git
   cd TrendRadar
   ```

无法访问 GitHub 的用户，我也上传了一个源码的压缩包，地址： https://pan.baidu.com/s/1WSAmH79EAqS7cdb16Cq4BQ?pwd=7qr3 。

2. **设置Secrets**:

在Docker环境下，可以通过环境变量或 `.env` 文件设置敏感信息，如：

```
# docker/.env
FEISHU_WEBHOOK_URL=https://...
AI_API_KEY=sk-xxxxxx
S3_ACCESS_KEY_ID=your-key
S3_SECRET_ACCESS_KEY=your-secret
```

3. **根据文档配置说明和实际需求配置 `config`文件夹下的配置**：

需要修改的配置如下：

* config/config.yaml - 功能配置（报告模式、推送设置、存储格式、推送窗口、AI 分析等）
* config/frequency_words.txt - 关键词配置（设置你关心的热点词汇）
* config/ai_analysis_prompt.txt - AI 提示词配置（自定义 AI 分析角色和输出格式，v5.0.0 新增）
* docker/.env - 敏感信息 + Docker 特有配置（webhook URLs、API Key、S3 密钥、定时任务）
* 💡 配置修改生效：修改 config.yaml 后，执行 docker compose up -d 重启容器即可生效

4. **本地构建**：
笔者修改了代码，于是构建自己的镜像。如果网络允许也可以参考[README.md](README.md)使用docker镜像构建：

```bash
# 克隆项目
git clone https://github.com/sansan0/TrendRadar.git
cd TrendRadar

# 修改配置文件
vim config/config.yaml
vim config/frequency_words.txt

# 使用构建版本的 docker compose
cd docker
cp docker-compose-build.yml docker-compose.yml 
```

仍然在`docker`目录下构建并启动服务，笔者启动了两个服务，即爬虫服务 + MCP AI分析服务：
```bash
docker compose build
docker compose up -d
```