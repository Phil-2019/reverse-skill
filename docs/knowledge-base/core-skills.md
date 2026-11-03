# reverse-skill 核心技能知识库

> 从 87 个技能中筛选出最核心的 **13 个**，覆盖架构骨架、高频分析、漏洞攻击、渗透侦察、闭环输出五大层。
> 依据：[路由优先级矩阵](skills/config/routing.json) + 实战触发频率 + 架构依赖关系。

---

## 一、架构骨架层

任何逆向/渗透任务都必须经过这两层，是所有其他技能的前置条件。

### 1. reverse-skill-router — 总控路由

**PRIMARY 入口**：`skills/SKILL.md` + `skills/MASTER-ROUTING.md`

**核心契约（读后 MUST 立即执行，不得只回复"已读"）**：

```text
1. 跑平台原生 router
   Windows: powershell -File skills\scripts\master-route.ps1 -Hint "<任务>"
   Linux:   bash skills/scripts/master-route.sh --hint "<任务>"
   → 从 config/routing.json 定 PRIMARY

2. 跑 case-init 落地 scope.md
   Windows: powershell -File skills\scripts\case-init.ps1 -Hint "<任务>"
   Linux:   bash skills/scripts/case-init.sh --hint "<任务>"
   → auth.status=granted 前禁止对目标 ACT

3. 立即打开 PRIMARY 的 SKILL.md → ACTION REQUIRED

4. 工具路径只认 tool-index.md；缺工具 → bootstrap（仅 manifest 能力，不猜路径）

5. 结论走 Evidence→Finding→Path
```

**优先级 Top 10（路由命中顺序）**：

| 优先级 | 触发条件 | PRIMARY |
|--------|----------|---------|
| R4 | DSL VM / 自定义 opcode VM | `reverse-engineering/dsl-vm-reverse/` |
| R1 | APK / smali / jadx | `apk-reverse/` |
| R2 | IPA / iOS / Objection | `mobile-reverse/` |
| R3 | JS 签名 / jshook / CDP | `js-reverse/` |
| R30 | 浏览器扩展逆向 | `browser-extension-reverse/` |
| R6 | IDA / 反编译深挖 | `ida-reverse/` |
| R17 | pwn / ROP / 堆栈利用 | `pwn-chain/` |
| R10 | 多阶段攻击链 | `attack-chain/` |
| R11 | Nmap / Nuclei / SRC | `pentest-tools/` |
| R0 | 通用逆向 / 未知二进制 | `reverse-engineering/` |

未命中任何强关键词 → fallback `R0`（通用逆向），并提示读 `skills/routing.md` 全矩阵。

**身份边界**：reverse-skill 是轻量路由包 + 工具自举 + journal，**不是** Z3r0 式平台。

---

### 2. ops 契约层 — scope / evidence / timeline

**位置**：`skills/ops/`

这层是安全红线和证据链，**不是可选的"流程"**，是硬性门禁。

**scope-contract.md — 授权门**

```text
auth.status != granted → 禁止对目标 ACT
network_profile 必须匹配实际网络：
  - authorized_target_only（只访问已授权目标）
  - isolated_lab（离线沙盒）
  - offline_sample（本地样本，offline-sample preset）
```

**evidence-finding-path.md — 证据链三要素**

```text
Evidence（可复现的原始记录：命令、输出、哈希）
  ↓ 归纳
Finding（分析结论：为什么这个证据意味着什么）
  ↓ 推导
Path（攻击路径 / 分析路径的完整链条）
```

**timeline-workitem.md — 时间线 + 任务项**

```text
timeline.md 记录每个 decision_delta + carry_forward_refs
workitems.md 覆盖阶段门闩，避免遗漏
```

**skill-supply-chain.md — 外部 skill/MCP 安装门闩**

```text
AST10 精简版：
  1. 读 README + SKILL.md 全文
  2. 检查 network access、下载 URL、postinstall 脚本
  3. sandbox 内跑一次，确认无异常网络
  4. 注册前显式告知用户副作用
```

---

## 二、高频分析类

这四个是日常命中率最高的分析技能。

### 3. R0 reverse-engineering — 通用逆向（兜底 + 方法论）

**位置**：`skills/reverse-engineering/SKILL.md`

**适用**：二进制 / APK / WASM / 固件 / 自定义 VM / 脱壳 / 反调试对抗 / 未知格式

**核心门闩**：`references/re-agent-workflow.md` 定义了四阶段：

```text
triage → static → dynamic → synthesis
   ↓
卡住时查 ops/analysis-decision-framework.md
  ADF-R43 deadlock/replan
  ADF-R4* evidence sufficiency
  ADF-R51 adversarial effort
```

**快速破案技巧（Try First!）**：

```bash
# 1. strings — 很多题 flag 明文
strings binary | grep -E "flag\{|CTF\{|secret|password"
rabin2 -z binary | grep -i "flag"

# 2. ltrace/strace — 动态直接抓 flag
ltrace ./binary
strace -f -s 500 ./binary

# 3. Frida hook strcmp/memcmp — 不等逆向直接抓 expected value
# 4. angr — 符号执行自动解 flag-checker
# 5. Qiling — 跨架构模拟 / 过反分析
```

**核心文档索引**（reverse-engineering/ 下）：

| 文件 | 内容 |
|------|------|
| `tools.md` | 静态分析工具总表 |
| `tools-dynamic.md` | Frida / angr / lldb / Triton |
| `tools-advanced.md` | VMProtect/Themida / D-810 / Mi asm / Rizin |
| `anti-analysis.md` | Linux + Windows 反调试全对抗 |
| `patterns.md` | 基础模式：XOR、自修改、opaque predicate |
| `patterns-ctf.md` / `-2.md` / `-3.md` | CTF 竞赛专项模式 |
| `languages-compiled.md` | Go / Rust / Swift / C++ 逆向 |
| `platforms.md` / `-hardware.md` | macOS / 固件 / 内核 / RISC-V / ARM64 |

**何时切换（Pivot）**：

```text
有漏洞点 → pwn-chain/
删文件/PCAP/磁盘 → digital-forensics/
Web 小客户端辅助 → js-reverse/
真恶意样本/C2 → malware-analysis/
多类型 CTF → ctf-sandbox/
```

---

### 4. R1 apk-reverse — Android APK 逆向

**位置**：`skills/apk-reverse/SKILL.md`

**适用**：Java 业务逻辑、签名/风控定位、smali 修改、重打包、Frida Hook、so/native 分析

**工具链**（当前机器已验证）：jadx 1.5.5 / apktool 3.0.2 / frida-ps 17.9.6 / adb / java

**高频脚本（优先用，不手写参数）**：

```bash
# 一次性解码 + 摘要输出
pwsh -File "skills/apk-reverse/scripts/decode.ps1" -ApkPath "app.apk" -Clean

# Frida 设备检查 + spawn/attach
pwsh -File "skills/apk-reverse/scripts/frida-run.ps1"

# 重建 + 对齐 + 签名 + 安装
pwsh -File "skills/apk-reverse/scripts/rebuild-sign-install.ps1"

# Manifest 关键组件与权限摘要
pwsh -File "skills/apk-reverse/scripts/manifest-summary.ps1"
```

**一行命令直接调（不封装）**：`adb devices` / `adb logcat` / `frida-ps -U` / `jadx --version`

**So 层切换**：Java 层分析完毕后发现 `.so` 内含关键逻辑 → 切 `ida-reverse` 或 `radare2`

---

### 5. R3 js-reverse — 前端签名逆向

**位置**：`skills/js-reverse/SKILL.md`

**适用**：接口签名、加密参数、风控字段、XHR/Fetch/WebSocket 触发点追踪、本地补环境复现

**核心原则（严格按顺序）**：

```text
Observe-first → Hook-preferred → Breakpoint-last → Rebuild-oriented → Evidence-first
```

先页面观察 → 再最小化采样 → 再本地补环境，**不要跳过取证直接猜环境**。

**五阶段工作流**：

```text
1. Observe    → list_scripts / search_in_sources / break_on_xhr
2. Capture    → evaluate_script / get_paused_info / list_network_requests
3. Trace      → get_request_initiator / set_breakpoint_on_text
4. Rebuild    → Node 补环境 / jsdom / canvas / localStorage 模拟
5. Evidence   → 输出 JSON 签名算法 + 参数位置 + 调用链
```

**jshookmcp 定位**：`js-reverse` 的增强执行面，不是独立总控。需要下载、显式注册 MCP server 后才可用。

**常用 MCP 工具映射**：

| 工具 | 用途 |
|------|------|
| `js-reverse_list_scripts` | 列出页面脚本 |
| `js-reverse_break_on_xhr` | XHR 断触发 |
| `js-reverse_evaluate_script` | JS 执行取证 |
| `js-reverse_get_request_initiator` | 追踪请求调用源 |

---

### 6. R6 ida-reverse — IDA Pro 深挖

**位置**：`skills/ida-reverse/SKILL.md`

**适用**：exe / dll / so / elf / macho / sys 等二进制的深度反编译、反汇编、数据流追踪、交叉引用

**已知坑（新人必看）**：

| 坑 | 解决 |
|----|------|
| MCP `idb_open` schema 校验报错 | 用 `scripts/open.ps1` 通过 HTTP API 直调 |
| System32 文件无权限打开 | open.ps1 自动复制到临时目录 |
| 启动服务器阻塞对话 | `scripts/start.ps1`（-WindowStyle Hidden） |
| RPC 超时留孤儿 worker 锁文件 | `start.ps1` managed supervisor；`opening.lock` 防并发 |
| HTTP MCP 静默退出 | `install-autostart.ps1` + watchdog 巡检 |
| Streamable HTTP GET /mcp 卡单线程 supervisor | `run-supervisor.py` 换 ThreadingHTTPServer |

**自带脚本**：

```bash
scripts/start.ps1          # 后台静默启动 idalib-mcp
scripts/open.ps1           # HTTP API 直开文件（自动处理 System32 + lock）
scripts/watchdog.ps1       # 每分钟巡检服务健康
scripts/recover.ps1        # Force 替换 supervisor
scripts/install-autostart.ps1  # 注册 Windows 计划任务
```

**MCP 服务器**：名 `idapro`，工具前缀 `idapro_*`；Remote HTTP 模式，`run-supervisor.py` 监听端口（默认 13337）。

---

## 三、漏洞攻击类

从理解程序到打通攻击的工程化桥梁。

### 7. R17 pwn-chain — 从漏洞点到 Working Exploit

**位置**：`skills/pwn-chain/SKILL.md`

**适用**：已经知道"哪里炸了"，需要写出稳定 exploit（不是本地复现一下就崩的脚本）

**前提**：本 skill **不负责发现漏洞**（那是 fuzzing / 审计），只负责"从漏洞点写出 exploit"。

**四步工作流**：

```text
Step 1: 确认漏洞类型 + 保护机制
   checksec ./vuln（NX / Canary / PIE / RELRO / Fortify）
   file + readelf → 漏洞分类：栈溢 / 格式化字符串 / 堆 / 整数 / 内核

Step 2: 选择利用策略
   NX关+无ASLR → 直接 shellcode
   NX开+给libc → ret2libc / one_gadget
   NX开+不给libc → leak 后 libc-database 反查
   堆 → 按 glibc 版本 (tcache / fastbin / unsorted / large)
   内核 → commit_creds / modprobe_path / core_pattern

Step 3: 准备 libc + gadget
   libc-database: ./find puts 0x6f0
   ROPgadget / Ropper / one_gadget

Step 4: 稳定化（CTF 本地通 → 远程打通的关键差距）
   libc 版本错配 / 堆喷射时序 / SMEP/SMAP/KASLR / 栈对齐 / 远程缓冲
```

**核心工具链**：pwntools + GEF/pwndbg + ROPgadget + one_gadget + libc-database + qemu-system

**分工**：

```text
识别 custom VM / anti-debug → reverse-engineering/
从零开二进制 → ida-reverse/ 或 radare2/
有漏洞点写 exploit → 本 skill
整合进完整攻击链 → attack-chain/（下游）
```

---

### 8. R10 attack-chain — 多阶段攻击编排

**位置**：`skills/attack-chain/SKILL.md`

**适用**："从 A 打到 B" 的完整链路 — HW 演练、外网到域控、钓鱼+后渗透、供应链跨组织多跳

**何时 MUST 经过本 Skill**：

| 场景 | 为什么要编排 |
|------|------------|
| 完整渗透测试 | 从信息收集到报告的全流程 |
| 外网 → 域控 | 突破→提权→横向→AD 多阶段 |
| HW 攻防演练 | 完整攻击链 + 隐蔽性 |
| 拿到 webshell 下一步 | 从据点规划后续路径 |
| Bug Bounty 持续监控 | 自动化多阶段流程 |

**单阶段任务不走这里**：

```text
只做端口扫描 → pentest-tools/
只做 SQL 注入 → pentest-tools/
只做 APK 逆向 → apk-reverse/
只做域渗透 → windows-ad/
```

**ACTION REQUIRED**（读完 MUST 执行）：

```text
1. 读 precedent-pentest.md — 确认是已授权常规操作
2. 创建/更新 case（case-init）+ scope.md
   auth.status != granted → 禁止 ACT
3. 以 lead 角色规划阶段（ops/role-map.md）+ specialist_roles
4. 每阶段更新 timeline.md + workitems.md
5. 结束时 docs-generator 报告必须含 Evidence 链
```

---

## 四、渗透侦察类

主动扫描 + 实战漏洞挖掘。

### 9. R11 pentest-tools — 主动渗透工具链

**位置**：`skills/pentest-tools/SKILL.md`

**适用**：信息收集 / 端口扫描 / 漏洞扫描 / Web 渗透 / SQL 注入 / 密码破解

**工具矩阵**：

| 类别 | 工具 | 典型场景 |
|------|------|---------|
| 信息收集 | Nmap / Masscan / Subfinder / httpx | 端口+服务+子域名+存活探测 |
| 漏洞扫描 | Nuclei / ZAP / Nikto | CVE 模板 / Web 扫描 |
| Web 渗透 | SQLMap / FFUF / Gobuster / XSStrike | SQLi / 目录爆破 / XSS |
| 密码破解 | Hashcat | GPU 哈希破解 |

**侦察管线门闩**（references/recon-pipeline.md）：

```text
scope 门（auth.granted + network_profile 合法）
  → 每批结果追加 timeline.md
  → 命中 ≠ 已验证，候选用 append-evidence.ps1 写入 Evidence
```

**现代 Web 客户端面**（DOM XSS / 原型污染）→ `references/client-side-lab-playbook.md`

**分工边界**：

```text
主动扫描/攻击 → 本 skill
逆向二进制 → ida-reverse/ 或 radare2/
前端 JS 签名逆向 → js-reverse/
浏览器/桌面自动化 → browser-automation/
```

---

### 10. src-hunter — 实战 SRC / 众测 / Bug Bounty

**位置**：`skills/pentest-tools/src-hunter/SKILL.md`

**适用**：SRC / 众测漏洞挖掘，实战工作流 skill（含 playbook 和 methodology）

**5 阶段方法论**：intake → recon → enum → hunt → report

**19 个攻击类 playbook**：SQLi / XSS / RCE / SSRF / IDOR / CSRF / Path Traversal / File Upload / Logic Flaw 等

---

## 五、闭环输出层

任务完成后必须经过的审查和交付。

### 11. R40 case-review — Evidence 图审查

**位置**：`skills/case-review/SKILL.md` + `skills/case-review/scripts/review_case.py`

**适用**：逆向 / 取证 / CTF / 授权安全 case 的可防御交接。只读审查，不改 case、不动目标。

**审查范围**：

```text
1. Scope 元数据 + 目标活动就绪
2. Evidence 记录结构 + 可复现命令
3. workitem / timeline 到 Evidence 的引用完整性
4. Finding + Path 在报告 Markdown 中的结构化
5. 可选 SHA-256 artifact hash 校验
```

**执行**：

```bash
python3 skills/case-review/scripts/review_case.py work/<case> --format markdown
python3 skills/case-review/scripts/review_case.py work/<case> --verify-hashes --strict
```

strict 模式下 scope warning 视为 handoff blocker。

**依赖**：仅 Python 3.9+ 标准库，**无需** bootstrap，无需网络。

---

### 12. R20 docs-generator — 报告生成

**位置**：`skills/docs-generator/SKILL.md`

**适用**：READMEs / API docs / 架构 docs / 逆向报告 / 渗透报告 / CTF writeup

---

## 六、特殊入口

### 13. R41 ctf-sandbox — CTF / AWD / 靶场 单入口

**位置**：`skills/ctf-sandbox/SKILL.md` → sidecar `CTF-Sandbox-Orchestrator/`

**适用**：CTF / AWD / 靶场（多类型编排），下游 42 个子技能分发

**42 个子技能**（精选 Top 10）：

| 子技能 | 场景 |
|--------|------|
| `competition-reverse-pwn` | 逆向 / DFIR / pwnable 综合 |
| `competition-web-runtime` | Web / API / SSR / Frontend |
| `competition-forensic-timeline` | DFIR 时间线交叉关联 |
| `competition-prompt-injection` | Prompt Injection / Retrieval Poisoning |
| `competition-reverse-pwn` | Reverse / Malware / DFIR / Pwnable |
| `competition-identity-windows` | AD / Kerberos / LDAP / OAuth |
| `competition-ssrf-metadata-pivot` | SSRF → 云元数据 |
| `competition-k8s-control-plane` | K8s API / Service Account |
| `competition-relay-coercion-chain` | Forced Auth / Relay Chain |
| `competition-request-normalization-smuggling` | Parser Differential / HTTP Smuggling |

---

## 附录：工作流全景

```text
用户任务 → router (master-route.ps1/.sh) → PRIMARY
                                         ↓
                                   case-init → scope.md
                                         ↓
                                    PRIMARY SKILL.md
                                         ↓
                            ┌────────────┼────────────┐
                            ↓            ↓            ↓
                       分析类       攻击类       渗透类
                    (R0/R1/R3/R6)  (R17/R10)   (R11)
                            ↓            ↓            ↓
                            └────────────┼────────────┘
                                         ↓
                              Evidence → Finding → Path
                                         ↓
                              case-review → docs-generator
                                         ↓
                              field-journal 回写经验
```

## 附录：工具 bootstrap 清单

```bash
# Windows PowerShell
pwsh -NoProfile -ExecutionPolicy Bypass -File "skills/scripts/bootstrap-reverse.ps1" -Capability @('jadx','frida','nmap','pwntools') -StartServices

# Linux / macOS / Kali
bash skills/scripts/bootstrap-reverse.sh jadx frida nmap pwntools --start-services
```

**manifest 能力**（以 `skills/scripts/bootstrap-manifest.json` 为准）：jadx / apktool / jeb-pro / binaryninja / frida / frida-ps / idalib-mcp / jshookmcp / idapro / r2 / rabin2 / adb / ghidra-mcp / nmap / pentestswarm / binwalk / yara / pwntools / bkcrack

JEB Pro 手动许可；Reqable MCP 手动安装客户端；unblob / EMBA 等**不**在 manifest，走手动安装。
