---
title: 使用 Github Action 建置 CI 流程
date: 2024-02-01
categories:
    - CI
tags:
    - Github Action
    - CI
weight: 10
---

## Github Action

是 GitHub 提供的一種持續整合（Continuous Integration，簡稱 CI）和持續部署（Continuous Deployment，簡稱 CD）的服務。它使開發者能夠在程式碼庫中定義和配置自動化的工作流程，這些工作流程可以在特定事件觸發時執行，例如: 推送程式碼（Push）、創建拉取請求（Pull）或發布新版本（Release）。

## 持續整合（CI）

是一種軟體工程流程，它通過自動化測試和程式碼檢查，確保新的程式碼能夠順利地整合到現有專案中。這有助於減少錯誤，提高程式碼質量，並加速軟體交付的過程。

## 範例

1. 先建立 Spring Boot 專案，並在根目錄下創建一個名為 .github/workflows 的目錄。GitHub Actions 將在這個目錄中查找工作流程配置文件。
    
```bash
mkdir -p .github/workflows
```

2. 在 .github/workflows 目錄中創建一個 YAML 文件，例如 ci.yml，用於定義你的工作流程。

```yml
name: Java CI with Maven

on:
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: maven

      - name: Run unit tests
        run: mvn -B test --file pom.xml

      - name: Build with Maven
        run: |
          mvn clean
          mvn -B package --file pom.xml

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Build and push
        uses: docker/build-push-action@v4
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/my-app:latest
```
以上流程主要目的是在程式碼推送到主分支時，構建 Java 應用程式，運行單元測試，並將構建的 Docker 映像推送到 Docker Hub。


3. 在專案的根目錄下創建一個名為 Dockerfile 的文件，該文件將用於定義如何構建 Docker 映像。

```Dockerfile
# Fetching 17 version of Java
FROM openjdk:17-oracle

# Setting up work directory
WORKDIR /app

# Copy the jar file into our app
COPY ./target/my-app-0.0.1-SNAPSHOT.jar /app

# Exposing port 8080
EXPOSE 8080

# Starting the application
CMD ["java", "-jar", "my-app-0.0.1-SNAPSHOT.jar"]
```
以上流程適用於 Spring Boot 應用程式，它將剛剛打包的 JAR 檔複製到容器中的 /app 目錄下。在容器內，它使用 OpenJDK 17 執行這個 JAR 檔。

4. 創建 Docker Hub 的 Secrets
* 到 https://hub.docker.com/settings/security 建立 Access Token
* 進入 GitHub 存儲庫頁面。
* 點擊存儲庫名稱下方的 "Settings" 選項卡。
* 在左側導航欄中，點擊 "Secrets and variables" 中的 "Actions"。
* 點擊 "New repository secret" 按鈕。
* 輸入 Secrets 名稱，例如 DOCKERHUB_USERNAME 和 DOCKERHUB_TOKEN。
* 分別輸入 Docker Hub 的 USERNAME 和 TOKEN，然後點擊 "Add secret" 按鈕來保存這些 Secrets。

![https://ithelp.ithome.com.tw/upload/images/20230917/20151559GbcW2rMWmD.png](https://ithelp.ithome.com.tw/upload/images/20230917/20151559GbcW2rMWmD.png)

5. 將本次變動內容上傳至 Github，就可以看到 Github Action 成功運行。


-----


*備註* 

在 Maven 專案設定中，可以看到 build 部分有一個 plugins 區塊，其中包含了 spring-boot-maven-plugin 插件的設定。這個插件用於建構 Spring Boot 應用程式，並會將生成的 jar 檔案命名為 ${artifactId}-${version}.jar。

```pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.15</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>my-app</name>
    <description>my-app</description>
    ... 以下省略
```

如果想要更改生成的 jar 檔案的名稱，可以在 build 部分的 plugins 區塊中配置 spring-boot-maven-plugin 插件的 finalName 屬性。例如，如果想要將生成的 jar 檔案命名為 my-app.jar，可以像這樣配置：

```pom.xml
... 以上省略
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <finalName>my-app</finalName>
            </configuration>
        </plugin>
    </plugins>
</build>
... 以下省略
```