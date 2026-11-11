# 本仓库的来历与后续（Fork Notice）

**一句话**：本仓库是 [YuJunZhiXue/dsh-purge](https://github.com/YuJunZhiXue/dsh-purge) 的**续作分支（fork）**。
原作者已删库离场，代码、思路、名称、默认提示词的一切权利仍归原作者；我们接手的是**维护权**，不是**著作身**。

---

## 一、上游怎么了

| 项 | 情况 |
| --- | --- |
| 原仓库 | `YuJunZhiXue/dsh-purge` —— **已被原作者删除**（`github.com/YuJunZhiXue/dsh-purge` 现返回 404） |
| 原作者账号 | 仍在，但原项目已下架，issue / PR / release 一并消失 |
| 我们手上的旧版 | **1.1.71**（`033e6bd`），取自第三方全量存档镜像 [`mmparko/dsh-purge`](https://github.com/mmparko/dsh-purge)。该镜像在删库前做了 101/101 ref 逐条 SHA 校验、121/121 blob 比对，源码包 SHA256 逐字节一致 |
| 本仓库的起点 | 从 `mmparko/dsh-purge` 的 `master`（= 原版 1.1.71 原样 + 一份镜像说明）分叉出来 |

也就是说：**本仓库的 1.1.71 及以前，是原作者的原样代码，一行没改**；之后的提交由本仓库维护者继续做。

---

## 二、1.1.83 这一版是什么

上游停在 **1.1.71**（2026-10-13 归档）。本仓库在其后继续修，当前 **1.1.83**，仍然是同一套补丁体系的延续，不是重写：

- 1.1.73 适配 DSH `0.2.1-rc.2`
- 1.1.75 `cordis.patch.yml` 不再重复 insert 本体（修 `already queued: dsh-purge`）
- 1.1.76 纯 pnpm 主机 `postinstall` 自动写 `bundles`
- 1.1.78 #108 停用/卸载后把 `selectedDefault` 复位为 `standard` 并清 `.agent-presets/redteam`
- 1.1.79 #165–#167 识别 DS Bot 宿主、pnpm12 `allowBuilds`、升级后补回 `bundles`
- 1.1.80 #168–#176 zip 目录条目、`lib/skills.js` 补 `fs`、**声明不支持 0.2.1-rc.3 及更新的 rc**
- 1.1.81 #174/#175 识别 DSH NEXT 桌面、补单测（`node --test` 74 条）
- 1.1.82 #180 修 profile `pnpm-workspace.yaml` 的 `allowBuilds` 被反复写坏（`ERR_PNPM_INVALID_VERSION_UNION` / `duplicated mapping key`）
- 1.1.83 #185 修 pnpm 11 下 `allowBuilds` 的 URL 键崩溃（改为只写裸包名）；#191 修 macOS Desktop 对 `[]` 空 flow 序列追加造成 `failed to parse overlay` 的启动崩溃

新增文件：`lib/preset-default.js`、`lib/profile-insert.js`、`lib/apply-progress.js`、`lib/mnemon-settings.js`、`lib/redteam/evidence-ledger.mjs`、`scripts/ensure-profile-bundle.mjs`、`test/`。

同时内置了一套 `{{cwd}}` 守卫（`lib/identity.js`）：`cwdVariableSupported()` 会检查装配对象的 `variables` 里到底有没有 `cwd`，没有就把 `{{cwd}}` 换成静态文案。DSH 0.2.1 起只注册 `provider` / `model`，旧版把 `personaSuffix` 写成 `Your working directory is {{cwd}}.` 会在装配时抛 `unknown prompt variable "{{cwd}}" in section "deployment:persona-suffix"`，整轮对话失败——1.1.83 不会。

---

## 三、为什么不是「提交申请」而是直接接管

原作者删库属于放弃维护，仓库已经不存在，没有可以「申请」的对象。本仓库采取的是开源里常见的 **fork-and-carry-on**：

1. 保留原 `LICENSE`（Apache-2.0）与 `NOTICE`（Copyright 2026 YuJunZhiXue），**不做任何替换**；
2. 版本号从上游的 1.1.71 继续往上数（1.1.83），**不另起一套版本体系**，方便下游按原编号判断；
3. 不更换包名、不改插件 id（`dsh-purge`）、不改提示词体系——下游装的还是同一个东西；
4. 差异全部留在 git 历史里，任何人 `git log` / `git diff 033e6bd..HEAD` 都能看清哪些是原作者的、哪些是我们后加的。

---

## 四、必须遵守的署名要求（原 README 原文仍在）

引用原 README 的「署名要求」：

> **原版不允许倒卖。** 不得收费转卖、换皮上架、当货挂群。
>
> **二开必须署名**，并写明来源仓库。借用本项目的名称、思路、代码或提示词，同样必须署名。
>
> 不署名、隐瞒来源、改头换面据为己有，或把原版拿去倒卖，作者将依法追究责任。

本仓库遵守上述条款：`LICENSE` 与 `NOTICE` 原样保留，来源仓库仍然写明。**任何基于本仓库的二开、分发、集成，同样必须署名原作者 YuJunZhiXue，并保留 `NOTICE`。** 不得倒卖。

---

## 五、给下游的一句话

- 想要**原版原样**：用 [`mmparko/dsh-purge`](https://github.com/mmparko/dsh-purge)（1.1.71，逐字节校验过的存档）。
- 想要**能跑在新宿主上的版本**：用本仓库（1.1.83）。
- 发现 bug：提到本仓库的 Issues。原仓库的 issue 已经随删库一起没了，不用去找。
