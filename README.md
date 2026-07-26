<div align="center">

# 网腾无限AI - 通识百科与万物解答专家

**基于 Vue 3 + Vite + Vanilla CSS 构建的 AI 通识百科与万物解答微应用，具备深色玻璃拟态自适应交互与微信端 H5 体验**

[Vue 3] · [TypeScript] · [Vite] · [Vanilla CSS] · [开源协议 MIT]

[![GitHub stars](https://img.shields.io/github/stars/WT-Agent/ai-baike?style=social)](https://github.com/WT-Agent/ai-baike)
[![GitHub license](https://img.shields.io/github/license/WT-Agent/ai-baike)](https://github.com/WT-Agent/ai-baike/blob/main/LICENSE)

[在线演示](#在线演示) · [快速启动](#快速启动) · [核心功能](#核心功能模块) · [评估指标](#ai-评估指标) · [参与贡献](#参与贡献)

</div>

## 关于我们

团队成员均来自 C9 等顶尖学府，在字节、腾讯、阿里的工程师组成，全职创业研发开源 AI 应用产品，让所有人感受 AI 的魅力。

本项目是网腾无限 AI 微应用的标准开发模版，内置了毛玻璃深色主题样式系统、移动端与 PC 端自适应响应式框架、API 中转代理配置与流量裂变逻辑。

**我们不搞概念，不卖课，只写能跑起来的代码。**

欢迎 Star、Fork、提 Issue，一起让这个项目变得更好用。

核心特性：
- **极简自适应交互**：提供毛玻璃质感的深色玻璃拟态自适应 Web 界面，高度适配移动端 H5 微信浏览器与 PC 体验。
- **一键零成本部署**：纯静态前端结构，支持零成本部署于 Vercel、GitHub Pages 或 CDN/OSS 静态托管服务。
- **安全开发代理**：本地开发支持使用个人 API 密钥发起代理请求，密钥由 Vite 服务器中转，无需担心前端泄露。
- **裂变解锁与留存**：内置微信朋友圈扫码分享拦截与额度重置机制，提升流量转化与留存。

## 核心功能模块

网腾无限AI 通识百科与万物解答专家为用户提供深入浅出、趣味盎然、逻辑严密的深度科普解读报告，包含以下四大核心功能模块：

1. **核心概念与一句话破题**：用最具吸引力、形象直观的语言揭示复杂科学现象背后的底层本质。
2. **生动比喻与深度原理解析**：运用贴近生活的类比拆解复杂的底层科学机制，降低认知门槛。
3. **冷知识拓展与趣味反常识**：深入挖掘颠覆常识认知、令人耳目一新的关联科普冷知识。
4. **思考延伸与生活应用举例**：提供与该现象相关的日常防坑提醒、实践指南、生活实验或启发性思考题。

## AI 评估指标

系统根据专业科普模型输出结果，自动生成包含 5 大维度的 AI 共识打分（1-5 分）：

1. **科学严谨度 (scientificRigor)**：评估回答中底层逻辑、实验依据与科学事实的准确性。
2. **科普易懂度 (popularizationClarity)**：评估语言表述的通俗易懂程度与大众可接受度。
3. **趣味吸引力 (interestFactor)**：评估内容的有趣程度、悬念设置与受众阅读兴趣。
4. **逻辑深度 (logicalDepth)**：评估知识因果推理链条的完整性与论述透彻性。
5. **比喻生动度 (analogyVividness)**：评估生活化类比对于抽象科学原理的映射契合度与解释力。

## 快速启动

### 1. 克隆项目
```bash
git clone https://github.com/WT-Agent/ai-baike.git
cd ai-baike
```

### 2. 安装依赖
项目强制使用 pnpm 作为包管理器：
```bash
pnpm install
```

### 3. 配置本地开发环境变量
复制并修改环境变量配置文件：
```bash
cp .env.example .env
```
根据微应用的功能类型，在 `.env` 中配置您的开发者密钥：
- `DEEPSEEK_API_KEY`: 您的 DeepSeek 开发者 API 密钥（用于文本生成任务）
- `DASHSCOPE_API_KEY`: 您的通义千问/通义万相开发者 API 密钥（用于多模态与生图任务）

### 4. 启动本地开发服务
```bash
pnpm dev
```
启动成功后在浏览器访问控制台输出的地址即可。

### 5. 生产构建打包
```bash
pnpm build
```
打包后生成的 `dist` 目录即为纯静态网页资源，可直接上传部署。

## 脚手架集成说明

本模板由私有总控仓库 `ai.wuxian.xyz` 中的 `@wuxian/cli` 脚手架统一管理，支持以下批量运维操作：

### 初始化或更新单个子项目

```bash
node bin/cli.js ai-baike
```

脚手架将自动：
1. 读取子仓库的 `README.md` 首行作为 Prompt 主题。
2. 注入 Vue 3 静态页面结构及标准配置文件。
3. 保留原有的 `.git` 配置与 `README.md`，不覆盖个性化内容。

### 批量同步所有子项目

```bash
node bin/cli.js all
```

将模板的最新变更（如 SSO 逻辑、额度控制）一键同步至全部 31 个子项目。

### Agent 配置维护接口

```bash
# 读取子项目配置
node bin/cli.js get ai-baike

# 写入/更新配置（支持热更新 prompt、model、title、temperature 等）
node bin/cli.js set ai-baike prompt "你是一位科普作家、科普短视频主创兼科学思维导师..."
node bin/cli.js set ai-baike model deepseek-chat
```

## 联系方式

- GitHub Issues: [提交反馈](https://github.com/WT-Agent/ai-baike/issues)
- 邮箱: us@wuxian.xyz

## 打赏支持

如果本项目对您有帮助，欢迎请作者喝杯咖啡。您的支持是持续维护与更新的动力。

<div align="center">

**微信支付** | **支付宝**
:---:|:---:
<img src="https://ai.wuxian.xyz/assets/tenpay.png" width="200" alt="微信支付"> | <img src="https://ai.wuxian.xyz/assets/alipay.png" width="200" alt="支付宝">

</div>

## 版权与许可

本项目基于 MIT License 开源协议。

Copyright (c) 2026. All rights reserved.
