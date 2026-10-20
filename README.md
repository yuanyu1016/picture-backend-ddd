<div align="center">

# 🖼️ Picture Backend DDD

**基于 Spring Boot 与领域驱动设计（DDD）的图片管理系统后端**

[![Java](https://img.shields.io/badge/Language-Java_8%2B-blue.svg?style=for-the-badge&logo=java)](https://github.com/yuanyu-1016/picture-backend-ddd)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.7.6-6DB33F.svg?style=for-the-badge&logo=springboot)](https://spring.io/projects/spring-boot)
[![Build](https://img.shields.io/badge/Build-Maven-C71A22.svg?style=for-the-badge&logo=apache-maven)](https://maven.apache.org/)
[![Architecture](https://img.shields.io/badge/Architecture-DDD-success.svg?style=for-the-badge)](https://github.com/yuanyu-1016/picture-backend-ddd)
[![Stars](https://img.shields.io/github/stars/yuanyu-1016/picture-backend-ddd?style=for-the-badge&color=yellow)](https://github.com/yuanyu-1016/picture-backend-ddd/stargazers)
[![Forks](https://img.shields.io/github/forks/yuanyu-1016/picture-backend-ddd?style=for-the-badge&color=orange)](https://github.com/yuanyu-1016/picture-backend-ddd/network/members)
[![Issues](https://img.shields.io/github/issues/yuanyu-1016/picture-backend-ddd?style=for-the-badge&color=red)](https://github.com/yuanyu-1016/picture-backend-ddd/issues)

[描述](#-描述) · [核心功能](#-核心功能) · [技术栈](#-技术栈) · [目录结构](#-目录结构) · [快速开始](#-快速开始) · [接口分组](#-接口分组) · [开发建议](#-开发建议)

</div>

---

## 📝 描述

基于 Spring Boot 与领域驱动设计（DDD）的图片管理系统后端。项目采用前后端分离架构，提供用户、图片、空间和团队协作等业务能力，并集成对象存储、图片搜索、AI 扩图、Redis、WebSocket 与 Knife4j 接口文档。

仓库地址：<https://github.com/yuanyu-1016/picture-backend-ddd>

---

## 🎯 核心功能

- 用户注册、登录、退出登录，以及基于 Sa-Token 的会话认证。
- 用户管理与管理员权限控制。
- 图片上传、URL 上传、批量上传、分页查询、编辑、删除和审核。
- 图片标签/分类管理、按图片搜索、按主色调搜索。
- 图片压缩、缩略图生成和腾讯云 COS 对象存储。
- 公共空间、私有空间和团队空间管理。
- 空间成员管理，以及 viewer/editor/admin 角色权限控制。
- 空间使用量、分类、标签、大小、成员和排行分析。
- 基于 WebSocket 的图片协同编辑消息广播。
- 基于 Redis 的 Session 存储和缓存能力。
- 基于 ShardingSphere 自定义算法的图片表分片代码；当前启动类显式排除了 ShardingSphere 自动配置，详见“已知限制”。
- 集成图片搜索接口和阿里云 DashScope 图片扩图任务接口。

---

## 🧰 技术栈

| 类别 | 技术 |
| --- | --- |
| 语言与运行时 | Java 8+ |
| Web 框架 | Spring Boot 2.7.6、Spring MVC、Spring WebSocket |
| 持久化 | MyBatis-Plus 3.5.9、MySQL |
| 认证与会话 | Sa-Token 1.39.0、Spring Session、Redis |
| 缓存 | Redis、Caffeine |
| 对象存储 | 腾讯云 COS |
| 分片 | Apache ShardingSphere 5.2.0 |
| 接口文档 | Knife4j OpenAPI 2 |
| 其他 | Lombok、Hutool、Disruptor、Jsoup |

---

## 📂 目录结构

```text
picture-backend-ddd/
├── src/main/java/com/yuanyu/picture/
│   ├── interfaces/       # 接口层：Controller、DTO、VO、Assembler
│   ├── application/      # 应用层：业务用例编排与事务边界
│   ├── domain/           # 领域层：实体、值对象、仓储接口和领域服务
│   ├── infrastructure/   # 基础设施层：数据库、COS、外部 API、配置和通用组件
│   ├── shared/           # 跨领域能力：认证、分片、WebSocket 等
│   └── PictureBackendApplication.java
├── src/main/resources/
│   ├── application.yml   # 通用配置和默认运行参数
│   ├── mapper/            # MyBatis Mapper XML
│   ├── biz/               # 业务配置，例如空间成员权限配置
│   └── static/            # 静态资源
├── src/test/              # Spring Boot 和 Redis 测试
├── sql/create_table.sql   # MySQL 初始化脚本
├── pom.xml                # Maven 构建与依赖配置
└── package.json           # OpenAPI 相关 Node.js 工具依赖
```

---

## ⚙️ 环境要求

- JDK 8 或更高版本
- Maven 3.6 或更高版本
- MySQL 8.x（项目使用 MySQL Connector/J）
- Redis 6.x 或更高版本
- 腾讯云 COS 存储桶及访问密钥
- 如果使用 AI 扩图功能，需要阿里云 DashScope API Key

---

## 🚀 快速开始

### 1. 获取代码

```bash
git clone https://github.com/yuanyu-1016/picture-backend-ddd.git
cd picture-backend-ddd
```

### 2. 初始化数据库

执行 [sql/create_table.sql](sql/create_table.sql)。脚本会创建 `picture` 数据库及 `user`、`picture`、`space`、`space_user` 表。

执行前请确认数据库账号具有建库、建表和修改表结构的权限。脚本包含多次 `ALTER TABLE`，不适合对已经执行过部分迁移的生产数据库重复执行。

### 3. 配置本地环境

项目默认激活 `local` profile。请在 `src/main/resources/application-local.yml` 中配置本地参数；该文件已被 `.gitignore` 忽略，不应提交密钥。

至少需要配置以下内容：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/picture
    username: your_mysql_user
    password: your_mysql_password
  redis:
    host: 127.0.0.1
    port: 6379

cos:
  client:
    host: https://your-bucket.cos.your-region.myqcloud.com
    secretId: your_tencent_secret_id
    secretKey: your_tencent_secret_key
    region: ap-guangzhou
    bucket: your-bucket-appid

aliYunAi:
  apiKey: your_dashscope_api_key
```

不要把真实的数据库密码、腾讯云密钥或 API Key 写入 Git。仓库中的 `application.yml` 仍包含本地开发默认值，部署或共享代码前应使用本地配置覆盖，并及时轮换已经暴露的凭据。

### 4. 构建与启动

```bash
mvn clean package
mvn spring-boot:run
```

应用默认监听 `8123` 端口，并使用 `/api` 作为上下文路径。启动后可访问：

```text
健康检查： http://localhost:8123/api/health
Knife4j：  http://localhost:8123/api/doc.html
```

也可以运行打包后的 JAR：

```bash
java -jar target/picture-backend-ddd-0.0.1-SNAPSHOT.jar
```

---

## 🔌 接口分组

所有 HTTP 接口默认带有 `/api` 前缀，Controller 的主要路径如下：

| 路径 | 说明 |
| --- | --- |
| `/api/user` | 注册、登录、用户信息和用户管理 |
| `/api/picture` | 图片上传、查询、编辑、审核、搜索和扩图任务 |
| `/api/space` | 空间管理和空间级别 |
| `/api/spaceUser` | 团队空间成员和角色管理 |
| `/api/space/analyze` | 空间使用量、分类、标签、大小、成员和排行分析 |
| `/api/file` | 文件测试上传/下载接口，管理员权限可用 |
| `/api/health` | 服务健康检查 |

图片协同编辑 WebSocket 地址为：

```text
ws://localhost:8123/api/ws/picture/edit
```

WebSocket 握手会经过登录用户校验，并按图片维度广播协同编辑消息。

---

## 🏗️ DDD 分层约定

- `interfaces` 只负责协议适配：接收 HTTP 参数、调用应用服务并组装响应。
- `application` 负责组织业务用例、事务和跨领域协作。
- `domain` 保持业务模型与仓储抽象，是核心业务规则的主要承载位置。
- `infrastructure` 提供 MyBatis、腾讯云 COS、第三方 API、缓存和配置等技术实现。
- `shared` 放置认证、权限、分片和 WebSocket 等跨多个业务模块使用的能力。

---

## 🧪 测试

执行全部测试：

```bash
mvn test
```

测试上下文会读取 Spring 配置，Redis 相关测试还需要可用的 Redis 服务；如果测试环境没有 MySQL、Redis、COS 或 DashScope 配置，请先准备对应的本地 profile，或按团队测试环境配置运行。

---

## ⚠️ 已知限制

- `PictureBackendApplication` 使用 `@SpringBootApplication(exclude = { ShardingSphereAutoConfiguration.class })` 排除了 ShardingSphere 自动配置，因此配置文件中的分片规则不会自动启用。项目保留了自定义分片算法和动态分片管理代码，启用分片前需要先完善启动配置与验证流程。
- `sql/create_table.sql` 是初始化脚本，不是版本化数据库迁移工具。生产环境请先备份并确认当前表结构，再执行增量变更。
- `node_modules` 当前历史上已被提交到仓库，正常开发仍应以 `package-lock.json` 为准，通过 `npm install` 恢复依赖；后续可单独清理 Git 历史中的依赖目录。

---

## 🤝 开发建议

```bash
# Java 构建
mvn clean package

# 安装 Node.js 工具依赖（仅在需要 OpenAPI 工具时）
npm install
```

提交代码前请确认：

- 不提交 `application-local.yml`、密钥、日志和构建产物。
- 新增业务逻辑放入合适的领域或应用层，避免在 Controller 中堆积业务规则。
- 数据库结构变更同步更新 SQL 或迁移说明。
- 对需要登录、管理员或空间角色权限的接口补充相应权限校验。

---

## 📄 许可证

当前仓库未发现 `LICENSE` 文件，因此 README 不声明具体开源许可证。若计划对外发布，请先补充许可证文件并同步修改本节。

---

<p align="center">
  <i>如果这个项目对你的开发或 DDD 学习有帮助，欢迎点一个 ⭐ 支持一下！</i>
</p>
