# Kuro-AutoSignin 项目分析

## 1. 项目概述
本项目是一个自动化脚本工具，用于管理库洛游戏（Kuro Games）相关的每日任务，主要包括库街区论坛任务和游戏签到任务。

## 2. 主要功能

### 2.1 库街区 (KuroBBS) 每日任务
脚本会自动执行以下库街区社区任务，以获取社区金币等奖励：
- **每日签到**：执行用户签到。
- **浏览帖子**：自动浏览列表中的帖子（默认浏览 3 篇）。
- **点赞帖子**：自动点赞帖子（默认点赞 5 次）。
- **分享帖子**：执行分享帖子的任务（1 次）。

### 2.2 游戏签到
支持以下游戏的每日签到和自动补签功能：
- **鸣潮 (Wuthering Waves)**
- **战双帕弥什 (Punishing: Gray Raven)**

**特性：**
- **自动获取奖励**：签到成功后自动获取奖励信息。
- **自动补签**：如果配置开启且有漏签，脚本会自动进行补签。
- **状态检测**：自动处理"已签到"、"用户信息异常"、"登录已过期"等状态。

### 2.3 其他功能
- **多账号支持**：通过 `config` 目录下的多个 `.yaml` 配置文件支持多账号管理。
- **推送通知**：支持通过推送服务（如企业微信、PushPlus 等）发送签到结果。
- **日志记录**：详细的运行日志，支持 debug 模式。
- **多平台部署**：支持 Windows/Linux 本地运行、Docker 容器、青龙面板、云函数（腾讯云/阿里云）。

## 3. 使用方法

### 3.1 环境准备
- 需要 Python 3.9 或更高版本。
- 安装依赖：
  ```bash
  pip install .
  ```

### 3.2 配置文件
1. 进入 `config` 目录。
2. 复制 `name.yaml.example` 为 `your_name.yaml`（文件名自定义）。
3. 编辑 `your_name.yaml`，填入关键信息：
   - `enable`: `true` (启用)
   - `token`: 你的库街区 Token (必填)
   - `auto_reple_sign`: `true` (是否开启自动补签)
   - 游戏角色信息（`distinct_id`, `devcode`, `wwroleId`, `eeeroleId`）可选填，脚本会尝试自动获取。

### 3.3 运行脚本
在项目根目录下执行：
```bash
python main.py
```
若需查看详细调试日志：
```bash
python main.py --debug
```

### 3.4 其他运行方式
- **Docker**: 使用提供的 `Dockerfile` 和 `docker-compose.yml` 构建运行。
- **青龙面板**: 订阅仓库 `https://github.com/mxyooR/Kuro-autosignin.git`，配置环境变量 `KuroBBS_config_path` 等。
- **云函数**: 配置入口为 `index.handler`，上传代码包并设置定时触发器。

## 4. 库街区每日任务详情确认
代码文件 `forum_sign_in.py` 中明确包含了以下逻辑：
- `do_task_view_posts()`: 循环浏览 3 篇帖子。
- `do_task_like_posts()`: 循环点赞 5 篇帖子。
- `do_task_share_post()`: 调用分享接口。

因此，**项目完全包含点赞、浏览、分享帖子等每日任务功能。**
