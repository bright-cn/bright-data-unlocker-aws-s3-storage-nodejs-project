# Bright Data Unlocker + AWS S3 Node.js 示例

[![Bright Data Promo](https://github.com/bright-cn/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/)

本项目演示如何使用 [Bright Data 的 Unlocker API](https://www.bright.cn/products/web-unlocker) 抓取网页内容，并将结果以 Node.js 的方式存储到 AWS S3 存储桶中。

https://github.com/user-attachments/assets/95b2dbe1-3612-471a-b8b1-95c578f0b8f8

## 功能
- 通过 Bright Data 的 Unlocker API 获取网页内容
- 将抓取的数据以 JSON 形式存储到 AWS S3
- 通过环境变量进行简易配置

## 先决条件
- Node.js（建议 v14 或更高）
- 具备 S3 访问权限的 AWS 账号（含凭据）
- Bright Data 账号

## 快速开始

### 1. 克隆仓库
```sh
git clone <your-repo-url>
cd <your-repo-directory>
```

### 2. 安装依赖
```sh
npm install
```

### 3. 配置环境变量
复制示例环境文件并填写你的凭据：
```sh
npm run init-env
```
然后编辑新创建的 `.env` 文件，设置实际的 API 密钥与配置项：

- `BRIGHT_DATA_API_TOKEN`：你的 Bright Data API token（[点此获取](https://www.bright.cn/cp/setting/users)）
- `BRIGHT_DATA_ZONE`：你的 Bright Data Unlocker 区域（zone）（[点此获取](https://www.bright.cn/cp/zones)）
- `AWS_S3_BUCKET`：你的 AWS S3 存储桶名称
- `AWS_REGION`：你的 AWS 区域（例如 `us-east-1`）
- `AWS_ACCESS_KEY_ID` 和 `AWS_SECRET_ACCESS_KEY`：你的 AWS 凭据（若未使用 IAM 角色）

### 4. 运行脚本
```sh
node index.js
```

## 工作原理
- 脚本通过 Bright Data 的 Unlocker API 请求目标 URL。
- 响应结果作为 JSON 文件保存到你指定的 S3 存储桶。
- 文件名带时间戳，并包含目标 URL 的哈希以保证唯一性。

## 自定义
- 根据需要在 `index.js` 或通过环境变量修改 `targetUrl` 或 `format`。
- 如需调整 S3 存储路径或元数据，可在 `uploadToS3` 函数中修改。

## AWS S3 用户角色权限

为使脚本能够向你的 S3 存储桶上传数据，你的 AWS 凭据（或 IAM 角色）必须具备相应权限。请使用 AWS IAM 创建并管理这些权限。

### IAM 策略示例

以下为授予向特定 S3 存储桶上传与读取对象权限的示例策略：

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

- 将 `YOUR-BUCKET-NAME` 替换为你的实际 S3 存储桶名称。
- 将此策略附加到你在 `.env` 中使用其凭据的 IAM 用户或角色。

更多详情请参见 [Bright Data 关于 AWS S3 用户角色权限的文档](https://docs.brightdata.com/datasets/scrapers/custom-scrapers/delivery-options#aws-s3-user-role-permissions)。
