# Static Resource Server

一个基于 FastAPI 的静态资源服务器，提供简单的文件访问功能。

## 功能特点

- 📁 **安全的文件访问** - 路径验证防止目录遍历攻击
- 🔧 **配置管理** - JSON 配置文件支持
- 🚀 **简单易用** - 一键启动，开箱即用

## 安装

### 环境要求

- Python 3.10+
- 依赖包（见 `requirements.txt`）

### 安装步骤

1. 克隆仓库

```bash
git clone https://github.com/qeggs-dev/static-resources-server.git
cd static-resources-server
```

2. 下载启动器

```bash
curl https://raw.githubusercontent.com/qeggs-dev/Sloves_Starter/refs/heads/main/Sloves_Starter.py -o run.py
```

## 快速开始

### 启动服务器

```bash
python run.py
```

### 配置文件

首次运行会在 `configs` 目录下生成默认配置文件 `default.json`：

```json
{
    "host": "0.0.0.0",
    "port": 8000,
    "base_path": "./static",
    "logger": {
        "file_path": "./logs/file-server-log-{time:YYYY-MM-DD_HH-mm-ss.SSS}.log",
        "level": "DEBUG",
        "rotation": "1 days",
        "retention": "7 days",
        "console_format": "<green>{time:YYYY-MM-DD HH:mm:ss.SSS}</green> | <level>{level: <8}</level> - <level>{message}</level>",
        "file_format": "{time:YYYY-MM-DD HH:mm:ss.SSS} | {level: <8} - {message}",
        "compression": "zip"
    }
}
```

#### 配置说明

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `host` | 服务器监听地址 | `0.0.0.0` |
| `port` | 服务器端口 | `8000` |
| `base_path` | 静态文件根目录 | `./static` |
| `logger.file_path` | 日志文件路径 | `./logs/file-server-log-{time:...}.log` |
| `logger.level` | 日志级别 | `DEBUG` |
| `logger.rotation` | 日志轮转周期 | `1 days` |
| `logger.retention` | 日志保留时间 | `7 days` |
| `logger.compression` | 日志压缩格式 | `zip` |

### 使用示例

#### 获取文件

```bash
# 获取图片
curl http://localhost:8000/images/photo.jpg

# 获取文本文件（指定编码）
curl http://localhost:8000/README.md?text_encoding=utf-8
```

## API 文档

### 获取文件

```
GET /{file_path}
```

**参数**

| 参数 | 类型 | 说明 |
|------|------|------|
| `file_path` | path | 文件路径（相对于 base_path） |
| `text_encoding` | query | 可选，指定编码以文本形式返回文件 |

**响应**

- 200 OK - 文件内容
- 400 Bad Request - 路径非法
- 404 Not Found - 文件不存在
- 500 Internal Server Error - 服务器内部错误

## 许可证

[MIT License](LICENSE)