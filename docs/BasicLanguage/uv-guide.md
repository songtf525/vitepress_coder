---

title: uv 使用指南（Python 包管理新一代工具）
description: 高性能 Python 包管理工具 uv 使用手册，从入门到实战
outline: deep
lastUpdated: true
-----------------

# ⚡ uv 使用指南（现代 Python 开发必备）

> uv 是由 Astral 推出的新一代 Python 包管理工具，目标是替代 pip + virtualenv + poetry，主打：**极致速度 + 简洁体验**

---

## 🧭 一、uv 是什么？

:::tip 一句话理解
**uv = pip + venv + poetry + pip-tools 的超级集合体**
:::

特点：

* 🚀 超快（Rust 实现）
* 📦 统一依赖管理
* 🧪 自动虚拟环境
* 🔒 锁定依赖（类似 poetry.lock）

---

## ⚙️ 二、安装 uv

```bash
curl -Ls https://astral.sh/uv/install.sh | sh
```

或：

```bash
pip install uv
```

检查版本：

```bash
uv --version
```

---

## 📦 三、创建项目（推荐方式）

```bash
uv init my_project
cd my_project
```

目录结构：

```
my_project/
├── pyproject.toml
├── uv.lock
```

---

## 🧪 四、虚拟环境（自动管理）

```bash
uv venv
```

激活：

```bash
source .venv/bin/activate
```

---

## 📥 五、依赖管理（核心）

### 安装依赖

```bash
uv add requests
```

### 安装开发依赖

```bash
uv add --dev pytest
```

### 删除依赖

```bash
uv remove requests
```

---

## 🔒 六、依赖锁定（非常重要）

```bash
uv lock
```

生成：

```
uv.lock
```

---

## 🚀 七、运行脚本

```bash
uv run python main.py
```

或直接：

```bash
uv run script.py
```

---

## 🔁 八、同步环境

```bash
uv sync
```

👉 根据 lock 文件恢复环境

---

## 🔥 九、替代 pip 用法

| pip           | uv        |
| ------------- | --------- |
| pip install   | uv add    |
| pip uninstall | uv remove |
| pip freeze    | uv lock   |
| virtualenv    | uv venv   |

---

## 🧠 十、实战工作流（推荐）

```bash
# 创建项目
uv init demo
cd demo

# 创建环境
uv venv

# 安装依赖
uv add fastapi

# 运行
uv run main.py
```

---

## ⚡ 十一、性能对比（核心优势）

| 工具     | 安装速度  |
| ------ | ----- |
| pip    | 慢     |
| poetry | 中     |
| uv     | 🚀 极快 |

---

## 💡 十二、最佳实践

:::tip 推荐

* 使用 uv 替代 pip
* 始终提交 uv.lock
* 用 uv run 执行脚本
  :::

:::warning 避坑

* 不要混用 pip 和 uv
* 不要删除 lock 文件
  :::

---

## 🎯 总结

> uv 的目标：

* 更快的依赖安装
* 更简单的开发体验
* 更一致的环境管理

---

## 📎 扩展学习

* 与 Poetry 对比
* CI/CD 集成
* Docker 环境使用

---

> ✨ 推荐全面迁移到 uv，Python 工程效率提升明显 🚀

---

## 🧪 十三、实战：uv + FastAPI（从0到部署）

### 1️⃣ 创建项目

```bash
uv init fastapi-demo
cd fastapi-demo
uv venv
```

### 2️⃣ 安装依赖

```bash
uv add fastapi uvicorn
```

### 3️⃣ 编写应用

```python
# main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello uv + FastAPI 🚀"}
```

### 4️⃣ 本地运行

```bash
uv run uvicorn main:app --reload
```

访问：

```
http://127.0.0.1:8000
```

### 5️⃣ 锁定依赖

```bash
uv lock
```

---

### 🚀 生产运行（推荐）

```bash
uv run uvicorn main:app --host 0.0.0.0 --port 8000
```

---

## 🐳 十四、uv + Docker 最佳实践（企业级）

### 📦 Dockerfile（推荐模板）

```dockerfile
FROM python:3.11-slim

# 安装 uv
RUN pip install uv

WORKDIR /app

# 复制依赖文件
COPY pyproject.toml uv.lock ./

# 安装依赖（利用缓存）
RUN uv sync --frozen

# 复制代码
COPY . .

# 启动服务
CMD ["uv", "run", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

### ⚡ 构建与运行

```bash
# 构建镜像
docker build -t uv-fastapi .

# 运行容器
docker run -p 8000:8000 uv-fastapi
```

---

### 🧠 Docker 优化建议（重点）

:::tip 最佳实践

* 先 COPY lock 文件 → 利用缓存
* 使用 `uv sync --frozen` 保证一致性
* 使用 slim 镜像减小体积
  :::

:::warning 避坑

* 不要在容器里用 pip
* 不要跳过 lock 文件
* 避免每次都重新安装依赖
  :::

---

### 🚀 进阶优化（可选）

#### 多阶段构建

```dockerfile
FROM python:3.11-slim AS builder
RUN pip install uv
WORKDIR /app
COPY pyproject.toml uv.lock ./
RUN uv sync --frozen

FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /app /app
COPY . .
CMD ["uv", "run", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## 🎯 本章总结

* uv + FastAPI：开发效率极高
* uv + Docker：部署简单且一致性强
* 核心：**lock + sync + run**

> 到这里，你已经具备“现代 Python 工程”的完整能力 🚀
