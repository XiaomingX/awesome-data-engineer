# 📅 数据建模指南

本项目包含第一周和第二周的数据建模模块设置。

### 🔧 **技术栈**

- Git  
- Postgres  
- PSQL CLI  
- 数据库管理工具（如 DataGrip、DBeaver、VS Code 扩展等）  
- Docker、Docker Compose、Docker Desktop  

---

## ✏️ **快速开始（TL;DR）**

1. [克隆项目仓库](https://github.com/DataExpert-io/data-engineer-handbook/edit/main/bootcamp/materials/1-dimensional-data-modeling/README.md)。  
2. [启动 Postgres 实例](https://github.com/DataExpert-io/data-engineer-handbook/edit/main/bootcamp/materials/1-dimensional-data-modeling/README.md#2%EF%B8%8F%E2%83%A3run-postgres)。  
3. 使用您喜欢的数据库管理工具[连接到 Postgres](https://github.com/DataExpert-io/data-engineer-handbook/edit/main/bootcamp/materials/1-dimensional-data-modeling/README.md#threeconnect-to-postgres-in-database-client)。  

如需详细操作说明，请参考下方逐步指南。

---

## 1️⃣ **克隆仓库**

- 使用 SSH 链接克隆项目，这将在本地创建一个新文件夹：

    ```bash
    git clone git@github.com:DataExpert-io/data-engineer-handbook.git
    ```

    > 💡 **提示**：建议使用 SSH 密钥与 GitHub 交互以增强安全性，设置方法详见 [官方文档](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)。  

- 进入克隆的项目目录：  

    ```bash
    cd data-engineer-handbook/bootcamp/materials/1-dimensional-data-modeling
    ```

---

## 2️⃣ **启动 Postgres**

您可以通过以下两种方法在本地运行 Postgres。

### 💻 方法 1：在本地直接运行

1. **安装 Postgres**：  
    - Mac 用户可参考 [教程](https://daily-dev-tips.com/posts/installing-postgresql-on-a-mac-with-homebrew/) 使用 Homebrew 安装  
    - Windows 用户可参考 [教程](https://www.sqlshack.com/how-to-install-postgresql-on-windows/)

2. 替换命令中的 **`<用户名>`** 后运行以下命令加载数据：  

    ```bash
    psql -U <用户名> postgres < data.dump
    ```

3. 配置 DataGrip、DBeaver 或 VS Code 扩展连接本地 Postgres 实例。  
4. 现在可以开始运行查询了！

### 🐳 方法 2：通过 Docker 启动

1. **安装 Docker Desktop**：[下载地址](https://www.docker.com/products/docker-desktop)  
2. 将 **`example.env`** 复制为 **`.env`**：  

    ```bash
    cp example.env .env
    ```

3. 启动 Docker Compose 容器：  
    - Mac 用户运行：  

        ```bash
        make up
        ```

    - Windows 用户运行：  

        ```bash
        docker compose up -d
        ```

4. 项目根目录将生成一个 **`postgres-data`** 文件夹，用于保存 Postgres 实例的数据。  

5. 检查容器是否运行：  
    - 打开 Docker Desktop 界面，确认 `postgres` 容器正在运行。  
    - 或运行以下命令查看容器状态：  

        ```bash
        docker ps -a
        ```

6. 停止 Postgres 容器：  
    - Mac 用户运行：  

        ```bash
        make down
        ```

    - Windows 用户运行：  

        ```bash
        docker compose down -v
        ```

---

## 3️⃣ **通过数据库管理工具连接 Postgres**

- 常用工具包括：  
    - **DataGrip**（JetBrains 出品，有 30 天免费试用）  
    - **VS Code** 扩展  
    - **PGAdmin**  
    - **Postbird**  

- 默认连接配置如下（可以通过 **`.env`** 文件修改）：  
    - 用户名：**`postgres`**  
    - 密码：**`postgres`**  
    - 数据库：**`postgres`**  
    - 主机：**`localhost`** 或 **`0.0.0.0`**  
    - 端口：**`5432`**

- 成功测试连接后，保存配置即可管理本地 Postgres 数据库。

---

## 🚨 **数据表加载失败怎么办？**

- 如果使用 Docker 启动 Postgres 时数据未正常加载，参考以下解决方案：  

1. 使用 Docker Desktop 打开容器终端，运行以下命令手动加载数据：  

    ```bash
    psql \
        -v ON_ERROR_STOP=1 \
        --username $POSTGRES_USER \
        --dbname $POSTGRES_DB \
        < /docker-entrypoint-initdb.d/data.dump
    ```

2. 或者，直接在命令行加载数据：  
    - 找到 `psql` 客户端的安装位置（如 `C:\\Program Files\\PostgreSQL\\13\\runpsql.bat`）  
    - 使用以下命令加载数据：  

        ```bash
        postgres=# \i data.dump
        ```

3. 如果仍有问题，可检查容器内数据文件并使用以下命令还原：  

    ```bash
    docker exec -it <容器ID或名称> bash
    pg_restore -U $POSTGRES_USER -d $POSTGRES_DB /docker-entrypoint-initdb.d/data.dump
    ```

---

### 💡 **其他 Docker 管理命令**

- 重启 Postgres 容器：  

    ```bash
    make restart
    ```

- 查看容器日志：  

    ```bash
    make logs
    ```

- 检查容器信息：  

    ```bash
    make inspect
    ```

- 查看 Postgres 运行端口：  

    ```bash
    make ip
    ```