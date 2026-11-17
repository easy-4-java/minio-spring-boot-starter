<a id="readme-top"></a>

<div align="center">

# minio-spring-boot-starter

**Spring Boot Starter，集成 minio**

[![Maven Central](https://img.shields.io/maven-central/v/io.github.easy4j/minio-spring-boot-starter)](https://github.com/easy-4-java/minio-spring-boot-starter)
[![Java](https://img.shields.io/badge/Java-17-orange)](#3-运行要求与兼容性)
[![License](https://img.shields.io/badge/license-Apache-2.0-green)](https://www.apache.org/licenses/LICENSE-2.0)

[English](./README.md) | [简体中文](./README.zh-CN.md)

[项目定位](#1-项目定位) · [核心能力](#2-核心能力) ·
[引入依赖](#5-引入依赖) · [快速开始](#6-快速开始) ·
[配置参考](#7-配置参考) · [版本线](#8-版本线与兼容性) ·
[构建测试](#9-构建与测试) · [许可证](#12-许可证)

</div>

---

> **当前版本**：`2.0.2-SNAPSHOT`<br>
> **JDK 基线**：`17`<br>
> **Group ID**：`io.github.easy4j`<br>
> **Artifact ID**：`minio-spring-boot-starter`<br>
> **许可证**：Apache License 2.0<br>

## 1. 项目定位

**minio-spring-boot-starter** 是一个面向 使用 minio 的应用 的 Spring Boot Starter，用于将 **minio** 集成到 Spring Boot 应用中。它提供自动装配、属性绑定与开箱即用的 Bean，使应用以最小配置即可使用 minio 的全部能力。

| 维度 | 说明 |
|---|---|
| 类型 | Spring Boot Starter |
| 消费方 | 使用 minio 的 Spring Boot 应用 |
| 核心能力 | 自动装配、属性绑定、开箱即用的 minio Bean |
| JDK | `17` |
| 坐标 | `io.github.easy4j:minio-spring-boot-starter:2.0.2-SNAPSHOT` |
| 配置前缀 | `minio` |

## 2. 核心能力

| 能力 | 状态 | 说明 |
|---|:---:|---|
| 自动装配 | ✅ 稳定 | 自动注册 minio 相关 Bean |
| 属性绑定 | ✅ 稳定 | 绑定 `minio.*` 到 `MinioProperties` |
| `MinioClient` Bean | ✅ 稳定 | 通过 MinioAutoConfiguration 自动注册 |

## 3. 运行要求与兼容性

| 依赖 | 最低版本 | 证据来源 |
|---|---:|---|
| JDK | `17` | `pom.xml` |
| Spring Boot | `2.6.0` | `pom.xml` parent |
| Maven | `3.6+` | Maven Enforcer |

## 4. 自动装配

Starter 自动装配以下 Bean：

| Bean | 条件 | 缺失时行为 |
|---|---|---|
| `MinioClient` | classpath + property | 不创建 |

自动装配注册：

- `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`（Spring Boot 2.7+ / 3.x / 4.x）
- `META-INF/spring.factories`（Spring Boot 2.x 传统方式）

## 5. 引入依赖

```xml
<dependency>
    <groupId>io.github.easy4j</groupId>
    <artifactId>minio-spring-boot-starter</artifactId>
    <version>2.0.2-SNAPSHOT</version>
</dependency>
```

无其他 easy4j 组件依赖。

## 6. 快速开始

### 6.1 引入依赖

在 `pom.xml` 中添加上述依赖。

### 6.2 配置

```yaml
minio:
  endpoint: http://localhost:9000
  access-key: ${MINIO_ACCESS_KEY}
  secret-key: ${MINIO_SECRET_KEY}
```

### 6.3 使用 Bean

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

Starter 根据以上属性创建 `MinioClient`。在应用服务中注入，例如：

```java
import io.minio.BucketExistsArgs;
import io.minio.MinioClient;

public class StorageService {
    private final MinioClient minioClient;

    public StorageService(MinioClient minioClient) {
        this.minioClient = minioClient;
    }

    public boolean bucketExists(String bucket) throws Exception {
        return minioClient.bucketExists(BucketExistsArgs.builder().bucket(bucket).build());
    }
}
```

请替换为自己的 MinIO 服务凭据，并通过环境变量提供密钥。`endpoint` 可以包含协议和端口；也可分别配置 `minio.port` 与 `minio.secure`。

## 7. 配置参考

### 7.1 配置前缀

`minio`

### 7.2 配置项

| 属性 | 类型 | 默认值 | 必填 | 说明 | 敏感 |
|---|---|---|:---:|---|:---:|
| `minio.endpoint` | string | 无 | 是 | MinIO 服务地址 | 否 |
| `minio.access-key` | string | 无 | 是 | 访问密钥 | 是 |
| `minio.secret-key` | string | 无 | 是 | 访问密钥密码 | 是 |
| `minio.port` | int | `-1` | 否 | endpoint 未包含端口时使用 | 否 |
| `minio.secure` | boolean | `false` | 否 | 单独配置主机和端口时使用 HTTPS | 否 |
| `minio.region` | string | 无 | 否 | 服务区域 | 否 |
| `minio.part-size` | long | 5 MiB | 否 | 分片大小，单位字节 | 否 |

## 8. 版本线与兼容性

| 分支 | JDK | Spring Boot | 组件版本 | 状态 |
|---|---:|---:|---|:---:|
| `2.3.x` / `2.7.x` | `8+` | 2.3.x / 2.7.x | `1.0.x` | 维护中 |
| `3.0.x` ~ `3.5.x` | `17` | 3.x | `2.0.x` | 维护中 |
| `4.0.x` / `4.1.x` | `17+` | 4.x | `3.0.x` | 活跃开发 |

## 9. 构建与测试

```bash
mvn clean verify
mvn -pl minio-spring-boot-starter -am test
```

## 10. 排障

| 症状 | 诊断 | 解决 |
|---|---|---|
| Bean 未创建 | 查看自动装配报告 | 确认 `minio.enabled=true` 与 classpath |
| `ClassNotFoundException` | 缺少依赖 | 引入对应模块 |
| 版本冲突 | `mvn dependency:tree` | 使用 BOM 统一版本 |

## 11. 贡献

1. Fork 本仓库。
2. 创建特性分支。
3. 提交前运行 `mvn clean verify`。
4. 提交 Pull Request。

## 12. 许可证

本项目采用 [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0) 许可证。

---

<div align="center">

[返回顶部](#readme-top) · [问题反馈](https://github.com/easy-4-java/minio-spring-boot-starter/issues) · [仓库地址](https://github.com/easy-4-java/minio-spring-boot-starter)

</div>
