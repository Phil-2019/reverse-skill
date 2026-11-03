# reverse-skill 技能提取报告

> 从仓库所有 `SKILL.md` frontmatter 中提取 name + description 汇总。

## 统计概览

| 维度 | 数量 |
|------|------|
| **总技能数** | 87 |
| `skills/` 主技能 | 45 |
| `CTF-Sandbox-Orchestrator/` 子技能 | 42 |

### `skills/` 分类

| 分类 | 数量 | 典型触发词 |
|------|------|-----------|
| 🔧 逆向工程 (reverse) | 20 | 逆向、反编译、IDA、Ghidra、APK、so、ELF |
| 🎯 渗透测试 (pentest) | 3 | 渗透、SRC、信息收集、攻击链 |
| 🛡️ 安全评估 (security) | 20 | API、云、K8s、AD、邮件、LLM |
| 📋 运维/文档 (ops) | 2 | Case 审查、报告生成 |

---

## 一、逆向工程类 (20 个)

| # | 技能名 | 路径 | 摘要 |
|---|--------|------|------|
| 1 | `apk-reverse` | `skills/apk-reverse` | 在 CLI 环境下做 Android APK 逆向时使用。适用于 APK 解包、Java 反编译、smali 修改、重打包、Frida 动态 Hook，以及按需切换到 so/native 分析。优先使用本机已安装的 jadx、apktool、frida、adb、ida-reverse、radare2。 |
| 2 | `binary-diff` | `skills/binary-diff` | 跨版本符号迁移与二进制差分。当你有旧版本的符号/逆向结果，需要快速迁移到新版本时使用。 适用场景：内核缺 PDB 用旧版符号推导、程序更新后批量迁移函数名、应用更新后快速定位新偏移。 核心方法：用 LLM 做结构化差异比对，程序化输入输出，成本极低（200 函数 ~1 元）。 触发关键词：符号迁移、bindiff、跨版本、PDB 缺失、函数偏移迁移、symbol migration、binary diff、版本对比。 |
| 3 | `binary-ninja-reverse` | `skills/binary-ninja-reverse` | Use for authorized binary analysis in Binary Ninja, including HLIL/MLIL/LLIL inspection, strings/imports/exports, cross-references, types, patch review, Python API automation, and optional Binary Ninja MCP or localhost HTTP integration. |
| 4 | `browser-extension-reverse` | `skills/browser-extension-reverse` | Use for authorized reverse engineering of browser extensions (Chrome/Firefox) including manifest analysis, background workers, and extension-based credential or traffic logic recovery. |
| 5 | `diagram-generator` | `skills/diagram-generator` | generate, refine, validate, and render diagrams from natural language, notes, code snippets, schemas, tables, or existing diagram source. use for flowcharts, swimlanes, sequence diagrams, state diagrams, er diagrams, class diagrams, architecture/c4-style diagrams, dependency graphs, gantt charts, mind maps, user journeys, sankey-style flows, org charts, network graphs, and other visual models. supports mermaid by default, graphviz dot for complex graph layout, plantuml for uml-heavy engineering diagrams, and svg output when direct markup is more reliable. |
| 6 | `dotnet-reverse` | `skills/dotnet-reverse` | .NET / C# 二进制逆向。当目标是 .NET assembly（PE 头含 CLR、.exe/.dll 托管程序）、C# 编译产物（含 NativeAOT）、红队 Sharp* 工具（Rubeus / SharpHound / SharpHound 等）、.NET 混淆程序（ConfuserEx / SmartAssembly / Babel / Eazfuscator）、.NET loader / info-stealer / 套壳 malware 时使用。优先用 dnSpyEx + de4dot，需要 AI 直接操作时联动 dnSpy MCP。不用于纯 native 二进制（走 reverse-engineering / ida-reverse）。 |
| 7 | `dsl-vm-reverse` | `skills/reverse-engineering/dsl-vm-reverse` | Reverse JavaScript-based custom DSL/VM interpreters, non-standard WASM-like runtimes, and risk-control engines. Use when analyzing IIFE or switch-based opcode dispatcher, stateful VM loops, custom instruction set with decoder, or obfuscation that turns real code into opaque dispatch tables. |
| 8 | `edr-bypass-re` | `skills/edr-bypass-re` | 逆向防御方实现 → 红队针对性绕过。把 EDR / Defender / AV 的 hook 表、ETW provider、AMSI 实现先逆向出来， 再写针对性的 unhook / 间接 syscall / ETW patch / AMSI bypass。 |
| 9 | `firmware-pentest` | `skills/firmware-pentest` | 固件 / IoT 渗透链。从拿到一坨 .bin / .img 开始，闭环走完逆向 → 提取 → 模拟 → 利用。 方法论遵循 OWASP FSTM 九阶段；工具链以 binwalk v3、unblob、EMBA、Firmadyne、AFL 为核心。 |
| 10 | `ghidra-reverse` | `skills/ghidra-reverse` | Use for free/open reverse engineering with Ghidra (headless or GUI), including decompile, cross-refs, and optional Ghidra MCP workflows when IDA is unavailable. |
| 11 | `go-rust-reverse` | `skills/go-rust-reverse` | Use for reverse engineering stripped Go and Rust binaries including runtime recognition, pclntab/moduel data recovery, panic strings, and idiomatic decompilation cleanup. |
| 12 | `ida-reverse` | `skills/ida-reverse` | IDA Pro 逆向分析辅助技能。当用户提到逆向、反编译、分析二进制/PE/ELF/APK/DLL/SO、破解、找密码、漏洞分析、病毒分析、firmware 固件分析，或需要分析 exe/dll/so/elf/macho/sys 等文件时，务必使用此技能。 |
| 13 | `js-reverse` | `skills/js-reverse` | 在使用 js-reverse-mcp 做前端 JavaScript 逆向时使用，适用于签名链路定位、页面观察取证、运行时采样、本地补环境复现与证据化输出。优先适配当前环境里的 js-reverse_* 工具，需要更强的浏览器/CDP/Hook 面时联动 jshookmcp。 |
| 14 | `macos-reverse` | `skills/macos-reverse` | Use for authorized macOS and Mach-O reverse engineering including codesign, Objective-C/Swift recovery, endpoint security surfaces, and Apple platform malware analysis. |
| 15 | `mobile-reverse` | `skills/mobile-reverse` | Use for authorized Android or iOS application reverse engineering and security testing, including APK or IPA analysis, runtime instrumentation, SSL pinning bypass, and anti-detection techniques. |
| 16 | `protocol-reverse` | `skills/protocol-reverse` | Use for authorized reverse engineering of custom binary protocols, Protobuf/gRPC, WebSocket frames, and PCAP-driven protocol recovery. |
| 17 | `pwn-chain` | `skills/pwn-chain` | 从逆向走到可用利用 (Working Exploit) 的全链路工程化方法。 覆盖栈溢、堆溢、Use-After-Free、格式化字符串、竞态、内核漏洞等主要方向。 |
| 18 | `radare2` | `skills/radare2` | Use this skill whenever the user wants to analyze binaries with radare2/r2 from the command line, including reverse engineering, disassembly, function analysis, and scripting. |
| 19 | `reverse-engineering` | `skills/reverse-engineering` | Provides reverse engineering techniques. Use when the main job is to understand how a compiled, obfuscated, packed, or virtualized target works before exploiting or solving it, including binaries, APKs, WASM, firmware, custom VMs, bytecode, malware-like loaders, and anti-debug or anti-analysis logic. |
| 20 | `thick-client` | `skills/thick-client` | Use for authorized security testing of desktop thick clients including local storage, update channels, IPC, traffic, and client-side trust boundaries. |

## 二、渗透测试 (3 个)

| # | 技能名 | 路径 | 摘要 |
|---|--------|------|------|
| 1 | `attack-chain` | `skills/attack-chain` | Use for authorized multi-stage attack-path planning and orchestration when a task spans reconnaissance, initial access, privilege escalation, lateral movement, and exfiltration across multiple hosts or environments. |
| 2 | `pentest-tools` | `skills/pentest-tools` | 主动渗透测试工具链。覆盖信息收集、端口扫描、漏洞扫描、Web 渗透、SQL 注入、目录爆破、密码破解等场景。 通过 MCP server（pentestMCP / mcp-security-hub）将 20+ 安全工具暴露给 AI agent。 |
| 3 | `src-hunter` | `skills/pentest-tools/src-hunter` | 实战 SRC / 众测 / Bug bounty 漏洞挖掘工作流 skill。包含：5 阶段方法论（intake → recon → enum → hunt → report）、19 个攻击类 playbook（SQLi/XSS/RCE/SSRF/IDOR/CSRF/Path Traversal/File Upload/Logic Flaw）。 |

## 三、安全评估 (20 个)

| # | 技能名 | 路径 | 摘要 |
|---|--------|------|------|
| 1 | `api-security` | `skills/api-security` | Use for authorized security assessment of REST, GraphQL, WebSocket, or SOAP APIs, including discovery, authentication, authorization, rate-limit, and CI/CD triggered testing. |
| 2 | `browser-automation` | `skills/browser-automation` | 统一自动化入口。覆盖浏览器自动化（Playwright）和 Windows 桌面应用自动化（OpenReverse）。 浏览器场景：打开网页、点击、填表、爬取、截图、自动化登录、渗透页面交互。 桌面场景：操作 IDA/x64dbg 等逆向工具的 GUI 自动化。 |
| 3 | `cloud-k8s` | `skills/cloud-k8s` | Use for authorized cloud, container, and Kubernetes security assessment including metadata SSRF, IAM misconfig, container escape paths, and cluster RBAC review. |
| 4 | `code-audit` | `skills/code-audit` | Use for authorized source-code security review and SAST workflows including Semgrep, CodeQL patterns, dangerous API hunting, and fix verification. |
| 5 | `ctf-sandbox` | `skills/ctf-sandbox` | Thin PRIMARY for CTF / AWD / 靶场 multi-type orchestration. Hands off to the sidecar CTF-Sandbox-Orchestrator. Use when the user says CTF, AWD, 靶场, or 比赛题 and the task spans multiple categories. |
| 6 | `database-security` | `skills/database-security` | Use for authorized database security assessment covering PostgreSQL/MySQL/MSSQL/Mongo/Redis exposure, authz, UDF/command paths, and misconfiguration review. |
| 7 | `digital-forensics` | `skills/digital-forensics` | Use for authorized digital forensics including memory dumps, disk timelines, PCAP investigation, artifact triage, and IR evidence preservation. |
| 8 | `email-security` | `skills/email-security` | Use for authorized email security review including phishing analysis, header authentication (SPF/DKIM/DMARC), BEC patterns, and mailbox token abuse research. |
| 9 | `hardware-security` | `skills/hardware-security` | Use for authorized hardware and embedded interface security research including UART/JTAG discovery, debug pad triage, secure boot overview, and offline firmware extraction. |
| 10 | `identity-federation` | `skills/identity-federation` | Use for authorized assessment of federated identity systems including SAML, OIDC, OAuth2 flows, SSO misconfiguration, and token confusion issues. |
| 11 | `llm-security` | `skills/llm-security` | Use for authorized security assessment of LLM applications and AI agents, including prompt injection, tool abuse, RAG exposure, memory poisoning, and model serving gaps. |
| 12 | `malware-analysis` | `skills/malware-analysis` | Use when analyzing suspected malware through static, dynamic, and behavioral techniques, including IOC extraction, YARA or Sigma rules, sandboxing, and anti-analysis bypass. |
| 13 | `ot-ics` | `skills/ot-ics` | Use for authorized OT/ICS security assessment covering Purdue model zoning, PLC/SCADA exposure, industrial protocol discovery, and safe passive-first evaluation. |
| 14 | `patch-diff-exploit` | `skills/patch-diff-exploit` | N-day 补丁差分到利用。从厂商发布的补丁里反推漏洞点、写 PoC、做成可用的攻击模块。 适用场景：已知 CVE 编号但只有补丁没有 PoC、SRC/红队需要打击未及时更新的资产、N-day 武器化、Patch Tuesday 跟进。 |
| 15 | `radio-sdr` | `skills/radio-sdr` | Use for authorized RF/SDR security research including signal identification, replay feasibility study in shielded labs, and wireless protocol analysis outside operational bands. |
| 16 | `supply-chain-security` | `skills/supply-chain-security` | Use for software supply-chain security assessment covering SBOM, SCA, CI/CD pipelines, container images, build integrity, dependency provenance, and vulnerability propagation. |
| 17 | `threat-hunting` | `skills/threat-hunting` | Use for blue-team threat hunting, detection engineering with Sigma/YARA, SIEM query design, and incident detection validation. |
| 18 | `threat-intelligence` | `skills/threat-intelligence` | Use for authorized OSINT and cyber threat intelligence that enriches IOCs, campaigns, impersonation, scams, or threat actors from public sources. Includes bounty intelligence patterns. |
| 19 | `wifi-wireless` | `skills/wifi-wireless` | Use for authorized wireless security assessment including Wi-Fi capture, WPA handshake analysis, rogue AP detection research, and lab-only deauth testing. |
| 20 | `windows-ad` | `skills/windows-ad` | Use for authorized Active Directory and Windows identity attacks including Kerberos, AD CS, BloodHound paths, NTLM relay, and domain privilege escalation research. |

## 四、运维/文档 (2 个)

| # | 技能名 | 路径 | 摘要 |
|---|--------|------|------|
| 1 | `case-review` | `skills/case-review` | Reviews a reverse-skill case package for scope readiness, Evidence to Finding to Path traceability, work item coverage, timeline references, and optional artifact integrity. |
| 2 | `docs-generator` | `skills/docs-generator` | Creates task-oriented technical documentation with progressive disclosure. Use when writing READMEs, API docs, architecture docs, or markdown documentation. |

---

## 五、CTF-Sandbox-Orchestrator 子技能 (42 个)

> 纯 CTF 多类型编排的子路由，由 `ctf-sandbox` 主技能分发。每个子技能都是 "Internal downstream skill"，只在 CTF sandbox 假设已建立后激活。

| # | 技能名 | 路径 | 适用 CTF 场景 |
|---|--------|------|--------------|
| 1 | `competition-ad-certificate-abuse` | `CTF-Sandbox-Orchestrator/competition-ad-certificate-abuse` | AD CS / ESC / 证书滥用 |
| 2 | `competition-agent-cloud` | `CTF-Sandbox-Orchestrator/competition-agent-cloud` | AI Agent / Prompt Injection / MCP |
| 3 | `competition-android-hooking` | `CTF-Sandbox-Orchestrator/competition-android-hooking` | Android Frida Hook / 运行时分析 |
| 4 | `competition-browser-persistence` | `CTF-Sandbox-Orchestrator/competition-browser-persistence` | 浏览器 Cookie / LocalStorage / 持久化 |
| 5 | `competition-bundle-sourcemap-recovery` | `CTF-Sandbox-Orchestrator/competition-bundle-sourcemap-recovery` | Source Map / Bundle 还原 |
| 6 | `competition-cloud-metadata-path` | `CTF-Sandbox-Orchestrator/competition-cloud-metadata-path` | SSRF → 云元数据路径 |
| 7 | `competition-container-runtime` | `CTF-Sandbox-Orchestrator/competition-container-runtime` | 容器运行时 / Mount / Escape |
| 8 | `competition-crypto-mobile` | `CTF-Sandbox-Orchestrator/competition-crypto-mobile` | 加密 / 编码 / APK 加密 |
| 9 | `competition-custom-protocol-replay` | `CTF-Sandbox-Orchestrator/competition-custom-protocol-replay` | 自定义协议回放 |
| 10 | `competition-dpapi-credential-chain` | `CTF-Sandbox-Orchestrator/competition-dpapi-credential-chain` | Windows DPAPI / 凭证链 |
| 11 | `competition-file-parser-chain` | `CTF-Sandbox-Orchestrator/competition-file-parser-chain` | 文件上传 / 解析链漏洞 |
| 12 | `competition-firmware-layout` | `CTF-Sandbox-Orchestrator/competition-firmware-layout` | 固件布局 / 分区表 |
| 13 | `competition-forensic-timeline` | `CTF-Sandbox-Orchestrator/competition-forensic-timeline` | DFIR 时间线交叉关联 |
| 14 | `competition-graphql-rpc-drift` | `CTF-Sandbox-Orchestrator/competition-graphql-rpc-drift` | GraphQL Schema / Persisted Query |
| 15 | `competition-identity-windows` | `CTF-Sandbox-Orchestrator/competition-identity-windows` | AD / Kerberos / LDAP / OAuth |
| 16 | `competition-ios-runtime` | `CTF-Sandbox-Orchestrator/competition-ios-runtime` | iOS Frida Hook / ObjC 运行时 |
| 17 | `competition-jwt-claim-confusion` | `CTF-Sandbox-Orchestrator/competition-jwt-claim-confusion` | JWT / JWS / JWE 声明混淆 |
| 18 | `competition-k8s-control-plane` | `CTF-Sandbox-Orchestrator/competition-k8s-control-plane` | Kubernetes API / Service Account |
| 19 | `competition-kerberos-delegation` | `CTF-Sandbox-Orchestrator/competition-kerberos-delegation` | Kerberos 委派 / SPN 信任 |
| 20 | `competition-kernel-container-escape` | `CTF-Sandbox-Orchestrator/competition-kernel-container-escape` | 内核攻击面 / Namespace Escape |
| 21 | `competition-linux-credential-pivot` | `CTF-Sandbox-Orchestrator/competition-linux-credential-pivot` | Linux 凭证 / Service 文件 |
| 22 | `competition-lsass-ticket-material` | `CTF-Sandbox-Orchestrator/competition-lsass-ticket-material` | LSASS / Windows 登录密钥 |
| 23 | `competition-mailbox-abuse` | `CTF-Sandbox-Orchestrator/competition-mailbox-abuse` | 企业邮箱滥用 / OAuth 同意 |
| 24 | `competition-malware-config` | `CTF-Sandbox-Orchestrator/competition-malware-config` | Malware C2 / Config 恢复 |
| 25 | `competition-oauth-oidc-chain` | `CTF-Sandbox-Orchestrator/competition-oauth-oidc-chain` | OAuth / OIDC / Redirect Flow |
| 26 | `competition-pcap-protocol` | `CTF-Sandbox-Orchestrator/competition-pcap-protocol` | PCAP 会话重放 |
| 27 | `competition-prompt-injection` | `CTF-Sandbox-Orchestrator/competition-prompt-injection` | Prompt Injection / Retrieval Poisoning |
| 28 | `competition-queue-worker-drift` | `CTF-Sandbox-Orchestrator/competition-queue-worker-drift` | 异步队列 / Worker / Cron |
| 29 | `competition-race-condition-state-drift` | `CTF-Sandbox-Orchestrator/competition-race-condition-state-drift` | 竞态窗口 / 排序 Bug / 幂等性 |
| 30 | `competition-relay-coercion-chain` | `CTF-Sandbox-Orchestrator/competition-relay-coercion-chain` | Forced Auth / Relay Chain |
| 31 | `competition-request-normalization-smuggling` | `CTF-Sandbox-Orchestrator/competition-request-normalization-smuggling` | Parser Differential / HTTP Smuggling |
| 32 | `competition-reverse-pwn` | `CTF-Sandbox-Orchestrator/competition-reverse-pwn` | Reverse Engineering / Malware / DFIR / Pwnable |
| 33 | `competition-runtime-routing` | `CTF-Sandbox-Orchestrator/competition-runtime-routing` | Reverse Proxy / Host Header / Forwarded |
| 34 | `competition-ssrf-metadata-pivot` | `CTF-Sandbox-Orchestrator/competition-ssrf-metadata-pivot` | SSRF / 内部路由探测 |
| 35 | `competition-stego-media` | `CTF-Sandbox-Orchestrator/competition-stego-media` | 图片 / 音频 / 视频隐写 |
| 36 | `competition-supply-chain` | `CTF-Sandbox-Orchestrator/competition-supply-chain` | CI/CD / Registry / 依赖漂移 |
| 37 | `competition-template-render-path` | `CTF-Sandbox-Orchestrator/competition-template-render-path` | SSR / 模板渲染 / Route Loader |
| 38 | `competition-web-runtime` | `CTF-Sandbox-Orchestrator/competition-web-runtime` | CTF Web / API / SSR / Frontend |
| 39 | `competition-websocket-runtime` | `CTF-Sandbox-Orchestrator/competition-websocket-runtime` | WebSocket / SSE 握手 / Auth |
| 40 | `competition-windows-pivot` | `CTF-Sandbox-Orchestrator/competition-windows-pivot` | Kerberos / WinRM / SMB / RDP 横向 |
| 41 | `competition-zip-archive` | `CTF-Sandbox-Orchestrator/competition-zip-archive` | ZIP / PKZIP 归档挑战 |
| 42 | `ctf-sandbox-orchestrator` | `CTF-Sandbox-Orchestrator/ctf-sandbox-orchestrator` | 默认入口 / CTF 编排主路由 |

---

## 六、路由优先级（由 `routing.json` 驱动）

> 优先级从高到低；未命中任何强关键词时 fallback 到 R0（通用逆向），并提示打开 `routing.md` 全矩阵。

| 优先级 | 触发条件 | PRIMARY 路径 |
|--------|----------|--------------|
| **R4** | DSL VM / fireye / 自定义 opcode VM | `reverse-engineering/dsl-vm-reverse/` |
| **R1** | APK / smali / jadx / apktool | `apk-reverse/` |
| **R2** | IPA / iOS / Objection / MobSF | `mobile-reverse/` |
| **R3** | JS 签名 / 前端加密 / jshook / CDP | `js-reverse/` |
| **R30** | 浏览器扩展逆向 | `browser-extension-reverse/` |
| **R31** | macOS / Mach-O | `macos-reverse/` |
| **R33** | Go / Rust 二进制 | `go-rust-reverse/` |
| **R5** | .NET / dnSpy / de4dot / ConfuserEx | `dotnet-reverse/` |
| **R9** | 恶意样本 / YARA / 沙箱 | `malware-analysis/` |
| **R21** | 协议 / Protobuf / PCAP 协议 | `protocol-reverse/` |
| **R22** | Ghidra / 开源反编译 | `ghidra-reverse/` |
| **R45** | Binary Ninja / HLIL / MLIL | `binary-ninja-reverse/` |
| **R6** | IDA / 反编译 / 反汇编深挖 | `ida-reverse/` |
| **R7** | radare2 / r2 | `radare2/` |
| **R8** | 固件 / binwalk / IoT / EMBA | `firmware-pentest/` |
| **R34** | 硬件调试口 / UART/JTAG | `hardware-security/` |
| **R28** | OT / ICS / 工控 | `ot-ics/` |
| **R17** | pwn / ROP / 堆栈利用 | `pwn-chain/` |
| **R16** | N-day / 补丁差分 | `patch-diff-exploit/` |
| **R18** | EDR / 免杀 / syscall | `edr-bypass-re/` |
| **R24** | Windows / AD / Kerberos / AD CS | `windows-ad/` |
| **R37** | 联邦身份 SAML/OIDC | `identity-federation/` |
| **R23** | 云 / 容器 / K8s | `cloud-k8s/` |
| **R35** | 数据库安全 | `database-security/` |
| **R25** | 取证 / 内存转储 / 时间线 | `digital-forensics/` |
| **R44** | OSINT / 威胁情报 | `threat-intelligence/` |
| **R36** | 邮件 / 钓鱼分析 | `email-security/` |
| **R29** | Wi-Fi / 无线渗透 | `wifi-wireless/` |
| **R38** | RF / SDR 研究 | `radio-sdr/` |
| **R32** | 厚客户端安全 | `thick-client/` |
| **R26** | 代码审计 / SAST / Semgrep | `code-audit/` |
| **R27** | 威胁狩猎 / 检测工程 | `threat-hunting/` |
| **R10** | 攻击链 / 红队 / 横向 | `attack-chain/` |
| **R11** | Nmap / Nuclei / SRC / 渗透工具 | `pentest-tools/` |
| **R12** | API / GraphQL / BOLA / JWT | `api-security/` |
| **R13** | SBOM / Trivy / 供应链 | `supply-chain-security/` |
| **R14** | LLM / Prompt 注入 | `llm-security/` |
| **R15** | bindiff / 符号迁移 / PDB | `binary-diff/` |
| **R19** | 浏览器/桌面自动化 | `browser-automation/` |
| **R40** | Case / Evidence 图审查 | `case-review/` |
| **R20** | 报告 / writeup | `docs-generator/` |
| **R39** | 图表 / Mermaid / PlantUML | `diagram-generator/` |
| **R41** | CTF / AWD / 靶场 (单入口) | `ctf-sandbox/` |
| **R0** | 通用逆向 / 反调试 / 未知二进制 | `reverse-engineering/` |

---

## 附录：工具链与运维文档位置

| 文档 | 用途 |
|------|------|
| `skills/MASTER-ROUTING.md` | PRIMARY 快路径与平台命令 |
| `skills/routing.md` | 全量三轴路由矩阵（目标类型/用户意图/工具链） |
| `skills/config/routing.json` | 路由唯一事实源（改路由只改这里） |
| `skills/INDEX.md` | 自动生成的导航索引 |
| `skills/tool-index.md.template` | 工具索引模板（首次激活后生成） |
| `AGENTS.md` | 仓库入口与激活边界 |
| `RULES.md` | 行为链唯一真相源 |
