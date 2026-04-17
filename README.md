# Redis-Keep-Alive
# Redis Cloud Keep-Alive 🌬️

> In the quiet garden of the cloud, some blossoms fade without attention. This simple action is the gentle breeze that keeps your free Redis instance vibrant and awake.

[![Redis Keep-Alive Workflow](https://github.com/<你的用户名>/<你的仓库名>/actions/workflows/redis-keep-alive.yml/badge.svg)](https://github.com/<你的用户名>/<你的仓库名>/actions/workflows/redis-keep-alive.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

###  evanescent dream of a free database

[Redis Cloud](https://app.redislabs.com/) 的免费套餐为开发者提供了绝佳的起点。然而，这份宁静的美好是短暂的——如果数据库在一段时间内无人问津，它便会陷入沉睡（被停用），等待着你手动将它唤醒。对于一个需要持续运行的应用来说，这无疑是一场易碎的梦。

### The eternal whisper

REDIS_URL_1 = rediss://default:密码A@hostA:端口A  Gmail
REDIS_URL_2 = rediss://default:密码B@hostB:端口B    
REDIS_URL_3

此项目利用 **GitHub Actions** 的力量，化作一道永恒的低语。它会按照你设定的节奏，定时向你的 Redis 数据库发送一个心跳信号，温柔地告诉云端：“我仍在此处，充满生机”。这确保了你的数据库永远不会因为闲置而沉睡。

### ✨ Features

*   **🕊️ 完全免费**: 运行在 GitHub Actions 的免费额度之上，无需任何服务器成本。
*   **⚙️ 极简设置**: 只需复制一个文件，设置一个密钥，三分钟即可完成部署。
*   **🧘 无需维护**: 一旦设置，它便会像时钟一样精准、安静地在后台工作。
*   **🕰️ 灵活调度**: 你可以随心所欲地定义心跳的频率，从每小时到每天。

---

### 🚀 Setting the Echo in Motion (Quick Start)

只需三步，即可为你的数据库注入不眠的活力。

#### Step 1: Obtain Your Redis URL

这是连接到你心灵花园的钥匙。

1.  登录到您的 [Redis Cloud 控制台](https.app.redislabs.com)。
2.  选择你的免费数据库，进入其 **"Configuration"** 页面。
3.  找到以下信息：
    *   **Public endpoint**: e.g., `redis-12345.c1.us-east-1-2.ec2.cloud.redislabs.com`
    *   **Port**: e.g., `12345`
    *   **Default user password**: 点击眼睛图标查看密码
4.  将它们组合成 `rediss://` 格式的 URL（注意是 `rediss`，代表 SSL 连接）：

    ```
    rediss://default:<你的密码>@<你的Public-endpoint>:<你的端口号>
    rediss://default:aVeryStrongPassword123!@redis-12345.c1.us-east-1-2.ec2.cloud.redislabs.com:12345
    ```

#### Step 2: Store the URL as a Secret

将这把珍贵的钥匙妥善保管在 GitHub 的保险箱中。

1.  在你的 GitHub 仓库中，导航到 `Settings` > `Secrets and variables` > `Actions`。
2.  在 **Repository secrets** 部分，点击 `New repository secret`。
3.  **Name**: `REDIS_URL`
4.  **Secret**: 粘贴你在上一步中组合好的完整 URL。
5.  点击 `Add secret` 保存。

#### Step 3: Create the Workflow File

这是施展魔法的卷轴。

1.  在你的仓库根目录下，创建文件夹 `.github/workflows/`。
2.  在该文件夹内，创建一个名为 `redis-keep-alive.yml` 的文件。
3.  将以下代码完整地复制粘贴到文件中：

    ```yaml
    name: Redis Keep-Alive

    on:
      schedule:
        # 默认设置为每 6 小时运行一次，拂晓、正午、黄昏、午夜，周而复始。
        # 你可以根据自己的需求调整 Cron 表达式。
        - cron: '0 */6 * * *'
      # 这也允许你随时手动触发此工作流，进行一次即时的唤醒。
      workflow_dispatch:

    jobs:
      ping-redis:
        runs-on: ubuntu-latest
        steps:
          # 使用一个稳定的 Python 版本，确保环境的一致性。
          - name: Set up Python
            uses: actions/setup-python@v4
            with:
              python-version: '3.11'

          # 安装与 Redis 通信所需的信使。
          - name: Install dependencies
            run: pip install redis

          # 发送那声温柔的问候。
          - name: Run Python Keep-Alive Script
            env:
              REDIS_URL: ${{ secrets.REDIS_URL }}
            run: |
              python -c "
              import redis, os, datetime
              print('Connecting to Redis...')
              r = redis.from_url(os.getenv('REDIS_URL'), decode_responses=True)
              timestamp = datetime.datetime.now().isoformat()
              r.set('github_actions:last_seen', timestamp)
              print(f'Whisper sent at: {timestamp}. Redis is awake.')
              "
    ```

4.  提交并推送这个文件。

一切就绪！从现在起，GitHub Actions 将会成为你数据库最忠实的守护者。你可以在仓库的 `Actions` 标签页中看到每一次“心跳”的记录。

---

### 🔧 Tuning the Frequency

想让问候更频繁或更稀疏？只需修改 `redis-keep-alive.yml` 文件中的 `cron` 表达式。

```yaml
- cron: '0 */6 * * *' # 每 6 小时
- cron: '0 0 * * *'   # 每天午夜
- cron: '0 */2 * * *' # 每 2 小时
```

你可以使用 [Crontab Guru](https://crontab.guru/) 来轻松生成你想要的任何时间表。

---

> May your data always be responsive and your projects forever alive. ✨
## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=gally16/Redis-Keep-Alive&type=Date)](https://www.star-history.com/#gally16/Redis-Keep-Alive&Date)
