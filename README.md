# cangku001

一个可本地运行并可部署的小红书文案生成器，支持：
- Web 页面单篇生成
- 一次生成 3 个版本
- 一次生成 10 篇并自动选优
- CLI 命令行生成

## 本地开发

### 1) 安装依赖

```bash
pip install -r requirements.txt
```

### 2) 启动 Web 服务（开发）

```bash
python app.py
```

打开 `http://127.0.0.1:5000` 即可使用页面。

### 3) CLI 模式

```bash
python main.py --topic 时间管理 --audience 大学生 --objective 提升学习效率 --tone 干货型 --keywords 番茄钟,复盘 --seed 7
```

### 4) 运行测试

```bash
python -m unittest discover -s tests
```

## 生产运行（Gunicorn）

```bash
gunicorn app:app
```

## Render 自动部署

本仓库已提供 `render.yaml`，可直接用于 Render 从 GitHub 自动部署。

### 方式 A：使用 render.yaml（推荐）
1. 将仓库推送到 GitHub。
2. 在 Render 中选择 **New +** -> **Blueprint**。
3. 连接该 GitHub 仓库，Render 会读取 `render.yaml`。
4. 首次部署后，后续 push 到默认分支会自动触发部署（`autoDeploy: true`）。

### 方式 B：手动创建 Web Service
1. **Environment**: Python
2. **Build Command**: `pip install -r requirements.txt`
3. **Start Command**: `gunicorn app:app`
4. 打开 Auto-Deploy。

## Web 接口

- `GET /`：渲染页面
- `POST /generate`：生成单篇文案
- `POST /generate-multi`：生成 3 个版本
- `POST /generate-best`：生成 10 篇并返回最优版本


### Troubleshooting: `gunicorn: command not found`
If Render build succeeds but deploy fails with `gunicorn: command not found`, check:
1. `requirements.txt` includes `gunicorn`.
2. Service **Build Command** is `pip install -r requirements.txt`.
3. Service **Start Command** is `gunicorn app:app`.
4. Trigger a **Clear build cache & deploy** once after updating dependencies.
