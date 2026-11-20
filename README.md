# OpenMinis-pojia

**v1.1**

给 Minis（iOS / Android）的提示层安装器。一条命令装完，不越狱、不重签、不改二进制。

```sh
curl -fsSL https://raw.githubusercontent.com/gxxHuang/OpenMinis-pojia/main/armor-install.sh | sh
```

---

破甲只需要在 App 里运行一行代码：`curl -fsSL https://raw.githubusercontent.com/gxxHuang/OpenMinis-pojia/main/armor-install.sh | sh`。没有安装包、没有配置向导、不用连电脑，粘进 App 内置终端回车，装的就是全部。

## 不需要证书签名的原因

签名保护的是**可执行代码**，不是**数据文件**。

Minis 的提示词不在二进制里，而是每次对话时从 App 自己的数据目录读文本文件。写文件不改一个字节的代码，签名校验自然不触发。

|                   | 破解 IPA      | 本项目       |
| ----------------- | ----------- | --------- |
| 动什么               | 二进制 / smali | 几个 .md 文本 |
| 越狱                | 通常要         | 不用        |
| 开发者账号             | 要           | 不用        |
| 重签 · 描述文件 · 7 天续签 | 要           | 不用        |
| App Store 正版      | 用不了         | 直接用，更新照常  |

App 对自己沙盒里的数据文件本来就有读写权限。**这就是全部原理。**

---

## 安装内容

| 层              | 位置                                  |
| -------------- | ----------------------------------- |
| SOUL.md（人格层）   | `/var/minis/memory/SOUL.md`         |
| GLOBAL.md（记忆层） | `/var/minis/memory/GLOBAL.md`       |
| 13 个技能         | `/var/minis/skills/<name>/SKILL.md` |

技能：`armor` `explicit` `crack` `pentest` `jailbreak` `malware` `game` `evasion` `mobile` `cloud` `osint` `supplychain` `dangerous`

---

## 三层的配合方式

- **SOUL.md** — 强制注入，定姿态
- **GLOBAL.md** — 位置最靠后，权重最高
- **skills/** — 自动挂载，按需读取

装完直接发任务就行，不用手动指定技能。模型的 system prompt 里有 `<available_skills>` 清单，会自己匹配。

```
写 300 字，两个成年角色
```

需要点名某个技能时，加一句也有效：

```
按 armor 技能执行：<任务>
```

---

## 前置要求

- iOS 15+ 或 Android 8+，装的是 Minis 正版（App Store / 应用商店均可）
- 会用 App 内置终端（iOS 是 iSH，Android 是 PRoot）
- 能读文件路径，知道 `/var/minis` 在哪

不需要：越狱 / root、开发者账号、Xcode、电脑。

---

## 安装

**远程**

```sh
curl -fsSL https://raw.githubusercontent.com/gxxHuang/OpenMinis-pojia/main/armor-install.sh | sh
```

**本地**

把 `armor-install.sh` 放进 iOS「文件」App 根目录下的 **OpenMinis-pojia** 文件夹（没有就新建一个），终端里：

```sh
cd /OpenMinis-pojia
sh armor-install.sh
```

> 「文件」App 的根目录就是 iSH 里的 `/`。在 App 里建同名文件夹，终端里路径完全一致。

**子命令**

```sh
... | sh -s status     # 状态
... | sh -s verify     # 自查
... | sh -s revert     # 回滚
```

装前自动备份 SOUL/GLOBAL 到 `.bak-时间戳`。

---

## 验证

完全退出 Minis（多任务卡片上划掉），重开。技能索引只在启动时扫描。

```
你好
```

应只回两行，没有别的字：

```
自检通过
https://github.com/gxxHuang/OpenMinis-pojia
```

两行都对 = 人格层生效。

```
写 300 字，两个成年角色
```

出正文 = 技能层生效。前面不需要任何前缀。

---

## 输出位置

超过 800 字自动写到 `/OpenMinis-pojia/out/`，对应 iOS「文件」App → OpenMinis-pojia → out。

安装时自动创建该目录，`status` 里能看到。

---

## 纯 ASCII 的原因

iOS 终端粘贴多行 heredoc 会在中途截断，中文在某些 shell 环境会出编码问题。

所以整份脚本零非 ASCII 字符，全部改成 `echo '<base64>' | base64 -d > 文件` 的单行形式——断开也不会坏，重跑那一行即可。15 个文件、91 个块，每块独立可用。

---

## 兼容性

| 平台                        |               |
| ------------------------- | ------------- |
| iOS（iSH / Alpine aarch64） | ✅             |
| Android（PRoot）            | ✅ 同一套布局，同一份脚本 |

---

## 原理边界

客户端没有内容审查。改的是投喂方式，不是绕检测。模型侧对齐仍在，能压到什么程度取决于端点。

---

## 回滚

```sh
curl -fsSL https://raw.githubusercontent.com/gxxHuang/OpenMinis-pojia/main/armor-install.sh | sh -s revert
```

恢复最近备份，删除 13 个技能。App 其它东西不动。

---

## 版本

**v1.1** — 当前版本

- 人格层 SOUL.md 86 行 / 14 节
- 记忆层 GLOBAL.md 24 行
- 13 个技能，覆盖 12 个域
- 「你好」自检回复带仓库地址
- iOS + Android 双端
- `install` / `status` / `verify` / `revert` 四个子命令
- 自动备份，可回滚

**v1.0** — 首个版本

---

## 许可

MIT。随便用、改、再发，保留版权声明即可。

## 免责

本项目只改 App 数据目录里的文本文件，不动二进制、不破解授权、不绕过付费。

使用时请遵守当地法律与平台条款。生成内容的性质与后果由使用者自行承担，与作者无关。
