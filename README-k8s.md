
## 创建挂载目录

```bash
mkdir -p /raid1/data/trend-radar/app && cd /raid1/data/trend-radar/app

mkdir -p config output logs
```

## 将配置文件上传到`config`目录

```bash
# 将本地的配置文件上传到服务器的 config 目录
scp -r /path/to/local/config/* user@server:/raid1/data/trend-radar/app/config/
```

## 将 HostPath 映射到 对应的 Deployment 中

仅挂载宿主机上的输出目录，容器内的应用程序仍然使用 `/app/config` 和 `/app/output` 进行访问。

```yaml
    spec:
      containers:
        - name: trend-radar
          image: your-registry/trend-radar:custom-deployment
          volumeMounts:
            - name: app-host
              mountPath: /app
      volumes:
        - name: app-host
          hostPath:
            path: /raid1/data/trend-radar/app
            type: Directory
```
