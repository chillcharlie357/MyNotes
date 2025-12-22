---
aliases: 
tags: 
categories:
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: Gemini
date:  2025-12-03 20:12
modified:  2025-12-22 18:12
---

# gemini配置

## 安装

### 安装Node

- mac/linux/wsl可使用nvm安装node
    - [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

```shell
wget -qOhttps://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash  
nvm install --lts
```

- windows：
    - 使用[nvm-windows](https://github.com/coreybutler/nvm-windows)
    - 或直接安装Node: [https://nodejs.org/en/download/current](https://nodejs.org/en/download/current)

### 安装gemini-cli

- 使用npm安装gemini-cli

```shell
npm install -g @google/gemini-cli
```

## 运行

- 在终端执行：

```shell
geimni
```

### API选择

![image-20251203194740581](file:///home/heleyang/Documents/notes/software_security/gemini.assets/image-20251203194740581.png?lastModify=1764765764)

|User Type / Scenario|Recommended Authentication Method|Google Cloud Project Required|
|---|---|---|
|Individual Google accounts|[Login with Google](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#login-google)|No, with exceptions|
|Organization users with a company, school, or Google Workspace account|[Login with Google](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#login-google)|[Yes](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#set-gcp)|
|AI Studio user with a Gemini API key|[Use Gemini API Key](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#gemini-api)|No|
|Google Cloud Vertex AI user|[Vertex AI](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#vertex-ai)|[Yes](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#set-gcp)|
|[Headless mode](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#headless)|[Use Gemini API Key](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#gemini-api) or [Vertex AI](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#vertex-ai)|No (for Gemini API Key) [Yes](https://github.com/google-gemini/gemini-cli/blob/main/docs/get-started/authentication.md#set-gcp) (for Vertex AI)|

#### 1. 使用Google账号登陆（最推荐）

- 免费套餐：每分钟 60 次请求，每天 1000 次请求
- Gemini 2.5 Pro，100 万令牌上下文窗口
- 无需管理 API 密钥 - 只需使用Google 帐户登录
- 自动更新至最新模型
- 需要配置`GOOGLE_CLOUD_PROJE`或`GOOGLE_CLOUD_PROJECT_ID`环境变量：
    1. 打开Google Cloud Console: `https://console.cloud.google.com`
    2. 新建一个项目或者选择一个已有的项目，复制项目ID
    3. 启用`Gemini for Google Cloud`服务
        - [https://console.cloud.google.com/marketplace/product/google/cloudaicompanion.googleapis.com](https://console.cloud.google.com/marketplace/product/google/cloudaicompanion.googleapis.com)
    4. 配置环境变量

# 方法1： 配置用户环境变量配置文件，如~/.bashrc  

```shell
echo 'export GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_ID"' >> ~/.bashrc  
source ~/.bashrc
```

# 方法2：使用.env文件  

```shell
mkdir -p ~/.gemini  
cat >> ~/.gemini/.env <<'EOF'  
GOOGLE_CLOUD_PROJECT="your-project-id"  

# Add other variables like GEMINI_API_KEY as needed  

EOF
```

## 2. 使用Gemini API Key

- 需要在环境便利配置API Key

```shell
# Get your key from https://aistudio.google.com/apikey  

export GEMINI_API_KEY="YOUR_API_KEY"  
gemini
```

## 3. 使用Vertex AI

- 需要配置环境变量

```shell
# Get your key from Google Cloud Console  

export GOOGLE_API_KEY="YOUR_API_KEY"  
export GOOGLE_GENAI_USE_VERTEXAI=true  
gemini
```