# HF Space Manager

HF Space Manager 是一个用于管理 Hugging Face Spaces 的 Web 应用程序。它提供了直观的界面和API 接口，让你能够方便地查看和管理多个账号下的所有 Spaces。

## 最近更新

2024-12-20 space 信息修改为访问 dashboard 时才会后台自动更新

## 功能特点

- 🚀 支持多个 HuggingFace 账号管理
- 📊 实时显示 Spaces 状态
- 🔄 支持重启和重建操作
- 🔑 安全的登录认证
- 🎯 简洁的用户界面
- 📱 响应式设计，支持移动端
- 🔌 RESTful API 支持
- 💾 数据缓存机制

## 界面预览
![index](/preview/index.png "index")

![dashboard](/preview/dashboard.png "dashboard")

## 快速开始

### 环境要求

- Python 3.9+
- Docker (可选)

### 安装

1. 克隆仓库
```bash
git clone https://github.com/ssfun/hf-space-manager.git
cd hf-space-manager
```

2. 安装依赖
```bash
pip install Flask python-dotenv huggingface_hub requests gunicorn
```

3. 配置环境变量
创建 `.env` 文件：
```env
USERNAME=your_admin_username
PASSWORD=your_admin_password
HF_TOKENS=token1,token2,token3
API_KEY=your_api_key
```

### 运行

**直接运行：**
```bash
python app.py
```

**使用 Docker：**
```bash
docker run -d \
    --name hfspace-manager \
    -p 5000:5000 \
    -e USERNAME=your_username \
    -e PASSWORD=your_password \
    -e HF_TOKENS=token1,token2,token3 \
    -e API_KEY=your_api_key \
    sfun/hfspace:latest
```

## API 文档

### 认证
所有 API 请求都需要在 header 中包含 API key：
```
Authorization: Bearer your_api_key
```

### 端点

#### 1. 获取所有 Spaces
```bash
curl -X GET "http://localhost:5000/api/v1/info/hf_token_here" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer API_KEY"
```

#### 2. 获取特定 Space 信息
```bash
curl -X GET "http://localhost:5000/api/v1/info/hf_token_here/username/space-name" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer API_KEY"
```

#### 3. 重启 Space
```bash
curl -X POST "http://localhost:5000/api/v1/action/hf_token_here/username/space-name/restart" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer API_KEY"
```

#### 4. 重建 Space
```bash
curl -X POST "http://localhost:5000/api/v1/action/hf_token_here/username/space-name/rebuild" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer API_KEY"
```

#### 5. 删除 Space
```bash
curl -X POST "http://localhost:5000/api/v1/action/hf_token_here/username/space-name/delete" \
-H "Content-Type: application/json" \
-H "Authorization: Bearer API_KEY"
```

## 配置说明

### 环境变量

| 变量名 | 说明 | 必填 |
|--------|------|------|
| USERNAME | 管理员用户名 | 是 |
| PASSWORD | 管理员密码 | 是 |
| HF_TOKENS | HuggingFace tokens（逗号分隔） | 是 |
| API_KEY | API访问密钥 | 是 |

### 缓存配置

- 访问 dashboard 时后台自动更新

## Docker 自动化构建

本项目设置了GitHub Actions自动化工作流程，可以自动构建Docker镜像并推送到GitHub Container Registry (ghcr.io)。

### 设置说明

1. GitHub Actions自动使用仓库的权限推送镜像到ghcr.io，无需额外设置密钥。
2. 确保仓库设置中已启用了GitHub Actions和包的读写权限。

3. 触发构建的方式：
   - 向`main`或`master`分支推送代码
   - 创建以`v`开头的标签（如`v1.0.0`）
   - 创建Pull Request到`main`或`master`分支
   - 手动在GitHub Actions页面触发工作流

### 镜像标签

自动构建的Docker镜像将使用以下标签策略：
- 分支推送：`ghcr.io/{username}/hf-space-manager:{分支名}`
- 标签推送：`ghcr.io/{username}/hf-space-manager:{版本号}`, `ghcr.io/{username}/hf-space-manager:{主版本号}.{次版本号}`
- 主分支推送：额外添加`ghcr.io/{username}/hf-space-manager:latest`标签
- 所有推送：添加`ghcr.io/{username}/hf-space-manager:sha-{提交哈希值}`标签

### 使用Docker镜像

```bash
docker run -d \
    --name hfspace-manager \
    -p 5000:5000 \
    -e USERNAME=your_username \
    -e PASSWORD=your_password \
    -e HF_TOKENS=token1,token2,token3 \
    -e API_KEY=your_api_key \
    ghcr.io/{username}/hf-space-manager:latest
```

请将`{username}`替换为您的GitHub用户名。

## 开发

### 本地开发
```bash
# 安装开发依赖
pip install Flask python-dotenv huggingface_hub requests gunicorn

# 运行开发服务器
python app.py
```

## 贡献

欢迎 Star 和 Fork 后自行修改！

## 许可证

本项目采用 [MIT 许可证](https://opensource.org/license/mit)。

## 作者

[ssfun](https://github.com/ssfun)

## 致谢

- [Hugging Face](https://huggingface.co/) - 提供优秀的 API 和服务
- [Flask](https://flask.palletsprojects.com/) - Web 框架支持
