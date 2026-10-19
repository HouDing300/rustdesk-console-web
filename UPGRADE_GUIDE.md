# RustDesk Console 定制改动与升级合并指南

> 用途：记录本次对 `databk/rustdesk-console`（后端）与 `databk/rustdesk-console-web`（前端）的定制改动，以及官方更新后如何合并而不丢失改动。
> 下次官方发布新版本时，把本文档交给 AI，让它按第 4 节执行。
> 最后更新：2026-10-19

---

## 0. 给未来的 AI：读完这份文档后请这样做

1. 读第 1 节确认仓库与分支基线；
2. 读第 2 节了解改动内容与改动原因（**不要用"照着上游重写"的方式处理这些文件**）；
3. 按第 4 节执行检查与合并，冲突时查第 5 节的逐文件决策表；
4. 合并后按第 4.5 节做验证，最后更新本文档第 1 节的"基线 commit / 版本"和最后更新日期。

---

## 1. 仓库与分支现状

| 项目 | 上游 | 我的 fork | 定制分支 | fork 时基线 | 基线版本 | 定制提交 |
| --- | --- | --- | --- | --- | --- | --- |
| 后端 | `databk/rustdesk-console` | `HouDing300/rustdesk-console` | `feat/device-last-ip-and-account` | `92cbd9fa66` | v1.10.0 | `645a43f3bc` |
| 前端 | `databk/rustdesk-console-web` | `HouDing300/rustdesk-console-web` | `feat/device-last-ip-and-account` | `ea623b96bf` | v1.7.0 | `5380676add` |

- 分支均为 GitHub 官方 fork（保留 upstream 关联），`main` 分支 = 上游纯镜像，不含任何定制改动。
- 定制分支各只有 **1 个提交**（后端 11 个文件 / 前端 7 个文件），方便 rebase。
- 两个仓库的定制分支**必须配套部署**：前端要展示 IP，后端必须已经提供 `info.ip`。

---

## 2. 本次改动要点

### 2.1 需求

1. 设备管理列表中展示：设备最后连接 IP、设备当前登录的计算机账号名、设备名称；
2. 搜索框支持按这三个字段搜索，且**直接展示搜索表单**（默认展开），不再藏在折叠区里。

### 2.2 调研得出的三个关键事实（决定了改动范围）

| 事实 | 含义 |
| --- | --- |
| `info.device_name`、`info.username` 后端**早已返回** | 前端原本把它们拼成一个 `username@device_name` 单元格，所以"设备名称/登录账号"主要是**前端拆列**工作 |
| `info.ip` 是**硬编码空字符串** `ip: ''`，全项目没有任何地方记录设备 IP | 这是唯一真正需要新建的后端能力（新字段 + 采集 + 查询） |
| `device_name` / `device_username` 的过滤后端**本来就有**（`getDevices()` 已实现） | 只是前端没把参数传过去，属于接线工作 |

另外：设备列表有两个数据源——设备页走 `/api/devices`（`device-group.service.ts`），设备选择弹窗走 `/api/peers`（`peer.service.ts`）。**两边都改了**，否则弹窗里 IP 会是空的。

### 2.3 后端改动（11 个文件）

| 文件 | 改动 |
| --- | --- |
| `src/common/entities/peer.entity.ts` | `peers` 表新增可空列 `lastIp`（`varchar(45)`） |
| `src/migrations/1792000000000-AddPeerLastIp.ts` | **新增**迁移，sqlite / mysql 双分支，带 `hasColumn` 幂等判断 |
| `src/database/data-source-options.ts` | 注册上述迁移（2 行：import + 数组项） |
| `src/modules/heartbeat/heartbeat.controller.ts` | 新增 `resolveClientIp(req)`：`X-Forwarded-For` 首跳 → `X-Real-IP` → `req.ip`；剥离 `::ffff:` 前缀，截断 45 字符 |
| `src/modules/heartbeat/heartbeat.service.ts` | `handleHeartbeat(data, clientIp?)`，新建 peer 与已存在 peer 都写入 `lastIp` |
| `src/modules/heartbeat/services/heartbeat-cache.service.ts` | `BufferedPeerUpdate` 增加可选 `lastIp`；flush 时**若 IP 未知则跳过该字段**（`lastIp === undefined ? baseUpdate : {...}`），避免把已有的 IP 抹成 NULL |
| `src/modules/device-group/dto/device.dto.ts` | `DeviceQueryDto` 增加 `ip`（模糊匹配） |
| `src/modules/device-group/dto/peer.dto.ts` | `PeerQueryDto` 增加 `device_name` / `device_username` / `ip` |
| `src/modules/device-group/device-group.service.ts` | `getDevices()`：select 加 `peer.lastIp`；返回 `info.ip`；新增 `ip` 过滤条件 |
| `src/modules/device-group/peer.service.ts` | `getAccessiblePeers()`：返回真实 `info.ip`；新增 `device_name` / `device_username` / `ip` 过滤 |
| `CUSTOM_CHANGES.md` | 仓库根目录新增的改动清单（英文） |

### 2.4 前端改动（7 个文件）

| 文件 | 改动 |
| --- | --- |
| `src/components/DeviceSelectTable/columns.tsx` | 把合并的 Device 单元格拆成三列：设备名称 / 登录账号 / 最后连接 IP，每列自带搜索框 |
| `src/components/DeviceSelectTable/index.tsx` | 向 `/api/peers` 透传 `device_name` / `device_username` / `ip` |
| `src/pages/devices/index.tsx` | `search={{ defaultCollapsed: false }}`（搜索表单默认展开）；request 透传三个新参数；CSV 导出补三列 |
| `src/services/rustdesk-console/typings.d.ts` | `DeviceListParams` / `AdminDeviceListParams` 增加 `ip` 及设备字段 |
| `src/locales/zh-CN/pages.ts` | 新增 `pages.devices.deviceName` / `deviceNameInfo` / `loginAccount` / `lastIp` 及三个搜索 placeholder |
| `src/locales/en-US/pages.ts` | 同上英文词条 |
| `CUSTOM_CHANGES.md` | 仓库根目录新增的改动清单（英文） |

### 2.5 IP 数据流（排障时按这条链路查）

```
设备 --POST /api/heartbeat--> 前端容器 nginx (proxy_set_header X-Forwarded-For)
   --> HeartbeatController.resolveClientIp()      # XFF 首跳 > X-Real-IP > req.ip
   --> HeartbeatService.handleHeartbeat(data, ip)
   --> heartbeat-cache 缓冲 --> flush 写 peers.lastIp
   --> getDevices() / getAccessiblePeers() 返回 info.ip
   --> 前端表格「最后连接IP」列 + ip 搜索参数
```

注意：后端**没有开启全局 `trust proxy`**，所以在控制器里手动解析转发头；nginx 已配置 `proxy_set_header X-Forwarded-For`，无需改 nginx。

---

## 3. 用到的东西

### 3.1 技术栈

| 层 | 技术 |
| --- | --- |
| 后端 | NestJS + TypeScript + TypeORM，支持 sqlite / mysql，心跳走缓冲批量 flush，设备在线状态基于心跳 |
| 前端 | Ant Design Pro（umi + `@ant-design/pro-components`，ProTable） |
| 部署 | Docker Compose（前端容器内置 nginx 反代 API），前端默认端口 21114，后端 3000 |
| 数据来源 | `peers`（新增 `lastIp`）、`sysinfos`（`hostname` → `info.device_name`，`username` → `info.username`） |

### 3.2 工具与环境

- Node 22（后端要求 ≥ 20，推荐 24）
- fork 用 **GitHub 官方 fork API**（服务端完成，保留完整历史与关联），不是本地推流
- 提交用 **GitHub Git Data API**（blob → tree → commit → ref 更新），单次提交
- 本地脚本：`push_fork.py`（工作区根目录，可复用于后续提交）

### 3.3 本机网络限制（重要，影响下次怎么干活）

| 端点 | 状态 |
| --- | --- |
| `api.github.com` | ✅ 通 |
| `codeload.github.com` | ✅ 通（下载源码压缩包） |
| `github.com`（git clone / push） | ❌ 被代理拦截（CONNECT 502） |

**结论：这台机器上 `git clone` / `git push` 一定失败。** 获取代码用 codeload 压缩包，提交用 API。第 4 节的 git 合并命令需要在**能正常访问 github.com 的机器**（你自己的开发机）上执行。

### 3.4 认证

- 使用 GitHub Personal Access Token（PAT，classic，全权限）。用完建议 revoke。
- WorkBuddy 内置 GitHub 连接器**没有 fork 权限**（`403 Resource not accessible by integration`），且 fork 端点要求 App 同时安装在源账号 `databk` 上——这条路结构性走不通，别再试。

---

## 4. 官方更新时：怎么检查、怎么合并

### 4.0 三条铁律

1. **`main` 永远是上游纯镜像**，绝不往 `main` 提交定制代码；
2. **定制改动只留在 `feat/device-last-ip-and-account`**，用 rebase 跟进上游，不要反复把 `main` merge 进来；
3. 我们只碰了第 2 节列出的那些文件。**如果在别的文件上出现冲突，说明上游挪了代码位置**，先回来看这张表，再决定把改动搬到新位置。

### 4.1 步骤一：先判断上游这次更新有没有碰到我们的文件

```bash
git fetch upstream
# 看看从基线到现在，上游改了哪些文件
git diff --stat <基线commit>..upstream/main
```

也可以直接看 release 的 changelog / milestone。

判断规则：
- 上游没碰第 2 节列表里的文件 → **直接 rebase，几乎必然干净**，跳过 4.4；
- 上游碰了 → 记录具体文件，到 4.4 查决策表。

### 4.2 步骤二：同步 main（在你自己的机器上执行）

```bash
# 后端（两个仓库分别执行，命令相同）
git remote add upstream https://github.com/databk/rustdesk-console.git   # 只需执行一次
git fetch upstream

git switch main
git merge --ff-only upstream/main     # 或: gh repo sync HouDing300/rustdesk-console -b main
git push origin main
```

若 `--ff-only` 失败，说明 `main` 被污染过（有人在 main 上提交了定制代码），必须先修正：把 main 重置回 `upstream/main`，定制提交 cherry-pick 到功能分支。

### 4.3 步骤三：rebase 功能分支

```bash
git switch feat/device-last-ip-and-account
git rebase main

# 有冲突时：解决 -> git add <file> -> git rebase --continue
# 想放弃这次 rebase：git rebase --abort

git push --force-with-lease origin feat/device-last-ip-and-account
```

### 4.4 步骤四：冲突文件逐文件决策表

| 上游改动的文件 | 我们的改动性质 | 冲突时的处理原则 |
| --- | --- | --- |
| `peer.entity.ts` | 新增 `lastIp` 列 | **保留两侧**。若上游也新增列，把 `lastIp` 补回去即可 |
| `1792000000000-AddPeerLastIp.ts`（新增文件） | 纯新增，上游不会有同名文件 | **直接保留**。若上游新增了迁移，检查时间戳顺序（我们是 1792000000000 ≈ 2026-10-14），迁移内有 `hasColumn` 幂等判断，顺序颠倒也不会出错 |
| `data-source-options.ts` | import + 数组项 2 行 | **保留两侧的注册项**，别只留一边 |
| `heartbeat.controller.ts` | 注入 `@Req()` 并解析 IP | 若上游重构了心跳接口签名，把 `resolveClientIp()` 私有方法搬过去，在 handler 里继续传第二个参数 |
| `heartbeat.service.ts` | `handleHeartbeat(data, clientIp?)` 第二参数 | 保留可选第二参数；内部写 `lastIp` 的两处（新建 / 已存在）都要在 |
| `heartbeat-cache.service.ts` | `BufferedPeerUpdate.lastIp` + flush 时跳过 undefined | **关键逻辑**：`lastIp === undefined` 时不写该字段。若上游改了 flush 逻辑，务必保留这个"跳过"语义 |
| `device.dto.ts` / `peer.dto.ts` | 新增查询参数 | 保留新增字段；若上游加了参数校验白名单（`forbidNonWhitelisted` 等），确认新字段在白名单内 |
| `device-group.service.ts` / `peer.service.ts` | `info.ip` 真实值 + 过滤条件 | 保留 `peer.lastIp` 的 select、返回与 `LIKE` 过滤。若上游重写了查询构造，按同样语义重新加回去 |
| `devices/index.tsx`（前端） | `defaultCollapsed: false` + 参数透传 + CSV 列 | 三处都要保留。若上游重写页面，把这三处补回新页面 |
| `DeviceSelectTable/columns.tsx`（前端） | 拆成三列 | 若上游重写列定义，按"三列 + 各自搜索框"重做 |
| `DeviceSelectTable/index.tsx`（前端） | 透传三个参数 | 保留透传 |
| `typings.d.ts`（前端） | 类型字段 | 若上游用 openapi 自动生成了该文件，**不要手动改生成文件**——改为在上层调用处断言，或把字段加进生成配置 |
| `locales/*.ts`（前端） | 新增词条 | 保留新增 key，冲突只是同文件相邻行，逐行保留即可 |

### 4.5 步骤五：验证

后端（改动后必做）：
```bash
npm install
npx tsc --noEmit -p tsconfig.build.json     # 类型检查
npm run build
```
启动后确认数据库迁移执行成功（`peers` 表出现 `lastIp` 列）。

前端：
```bash
npm install
npm run build                                # 本次未跑通完整类型检查，务必跑一次
```

功能冒烟：
1. 任意设备上线并发送心跳（或等待下一次心跳）；
2. 设备管理页出现「设备名称」「登录账号」「最后连接IP」三列，IP 有值；
3. 三个搜索框可直接输入过滤（表单默认展开）；
4. 打开"选择设备"弹窗，同样能看到并搜索这三列；
5. CSV 导出包含这三列。

### 4.6 步骤六：部署

- 后端与前端**同时**升级（先后端，再前端）；
- 历史设备在下次心跳前 IP 为空，属正常现象；
- 升级后确认 `peers.lastIp` 未被回填为 NULL（若发现被清空，检查 `heartbeat-cache.service.ts` 的"跳过 undefined"逻辑是否还在）。

---

## 5. 已知限制与风险

| 项 | 说明 |
| --- | --- |
| IP 生效时机 | 只有部署后发过心跳的设备才有 IP；历史设备需等下一次心跳 |
| 前端完整类型检查未跑通 | 本次 `npm install` 在沙箱内失败（umi 依赖体积导致 ENOSPC/-4048），只做了语法级检查。本地 build 时请确认 |
| 迁移时间戳 | 我们用的是 1792000000000（≈2026-10-14）。若上游后续新增同时间段迁移，typeorm 按文件名排序执行，迁移内部有 `hasColumn` 幂等判断，不会出错，但建议届时确认执行顺序 |
| IPv6 | IPv6 映射地址去掉 `::ffff:` 前缀后存储，长度上限 45 字符 |
| 安全 | 本次使用的 PAT 为全权限 classic token 且出现在会话中，建议 revoke 后改用最小权限 fine-grained token |

---

## 6. 下次让我干活时，可以直接复制这段话

```
官方文档有更新了。请先读 UPGRADE_GUIDE.md（本工作区根目录；两个 fork 仓库的
feat/device-last-ip-and-account 分支里也各有一份），按第 4 节执行：
1. 对比上游从 <基线commit> 到最新 main 的改动，列出是否触碰了文档第 2 节列出的文件；
2. 给出逐文件的冲突风险评估（对照第 5 节决策表）；
3. 输出我需要在自己机器上执行的完整合并命令（本机 github.com 被代理拦截，git 不可用，
   只能给命令让我本地跑，或用 API 方式修改文件）；
4. 合并后告诉我该跑哪些验证。
最后请更新 UPGRADE_GUIDE.md 第 1 节的基线 commit / 版本与最后更新日期。
```

---

## 7. 回滚

```bash
# 功能分支回退到改动前（定制提交只有 1 个）
git switch feat/device-last-ip-and-account
git reset --hard <基线commit>
git push --force-with-lease origin feat/device-last-ip-and-account

# 数据库（如需彻底移除字段）
#   迁移 down() 会 drop peers.lastIp；或直接 ALTER TABLE peers DROP COLUMN lastIp
```

前端回滚后表格会恢复成 `username@device_name` 合并单元格，与后端无关，可独立回滚。

---

## 8. 验证环境部署记录（192.168.51.91，Rocky Linux 9.8）

官方推荐 Docker Compose，但该机器 **Docker Hub 不可达**（`registry-1.docker.io` 连接超时），
且 compose 里的镜像是官方预构建镜像（不含定制改动），所以采用**原生部署**：

| 项 | 位置 / 值 |
| --- | --- |
| 后端源码 | `/opt/rustdesk-console`（定制分支 tarball 解压） |
| 前端源码 | `/opt/rustdesk-console-web`，构建产物 `dist/` |
| Node | `/opt/node22`（v22.22.2，软链到 `/usr/local/bin`） |
| 后端服务 | systemd `rustdesk-console`，`node dist/main.js`，监听 3000 |
| web | nginx（`/etc/nginx/conf.d/rustdesk-console.conf`），静态 + `/api/` 反代到 3000 |
| 数据库 | sqlite，`/opt/rustdesk-console/data/rustdesk-console.db` |
| 环境变量 | `/opt/rustdesk-console/.env`（`PORT` / `JWT_SECRET` / `ADMIN_PASSWORD` / `DB_TYPE=sqlite` / `DATA_DIR`） |
| 访问地址 | http://192.168.51.91/ |

注意点（下次重装/迁移时有用）：

- 默认管理员用户名是 **`databk`**（不是 admin），密码取自 `ADMIN_PASSWORD`。
- 首次启动会报 `Database migrations are pending`，先跑一次：
  `cd /opt/rustdesk-console && node dist/main.js migrate`，再 `systemctl restart rustdesk-console`。
- `npm install` 需要编译 `sqlite3` → 必须先装 `gcc gcc-c++ make python3`。
- SELinux 为 Enforcing：需要 `setsebool -P httpd_can_network_connect 1`（nginx 反代）
  和 `chcon -R -t httpd_sys_content_t <dist>`；防火墙需放行 http。
- 该机器 `git clone` 走 443 会超时，改用 codeload 下载 tarball：
  `https://codeload.github.com/HouDing300/<repo>/tar.gz/refs/heads/feat/device-last-ip-and-account`
- 快照恢复后系统时钟会回退（曾落后 48 天），导致 dnf 报证书未生效；
  先 `hwclock --hctosys` 再装包。

验证结果（2026-10-19）：模拟心跳带 `X-Forwarded-For: 203.0.113.7`，
`/api/devices` 与 `/api/peers` 均返回 `info.ip=203.0.113.7`；
`device_name` / `device_username` / `ip` 三个搜索参数过滤均正确。

## 9. 命令速查

```bash
git remote add upstream https://github.com/databk/rustdesk-console.git        # 后端
git remote add upstream https://github.com/databk/rustdesk-console-web.git    # 前端

git fetch upstream
git switch main && git merge --ff-only upstream/main && git push origin main
git switch feat/device-last-ip-and-account && git rebase main
git push --force-with-lease origin feat/device-last-ip-and-account

# 查看我们的改动相对上游有多少提交、改了哪些文件
git log --oneline main..feat/device-last-ip-and-account
git diff --stat main..feat/device-last-ip-and-account
```
