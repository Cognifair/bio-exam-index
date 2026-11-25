# 高考生物真题卡 · 公开引用索引

**Cognifair** · 墨墨记忆卡牌组《【🔥27高考生物】5年真题 · 各地选择题分类全刷》

1097 道真题（2022–2026，85 套卷）/ 218 个考点 / 25 章，全部免费公开引用。

## 给 Agent 的入口（纯 JSON，无鉴权）

| 用途 | URL |
|---|---|
| 主索引（考点 → root_id，约 116 KB） | `https://cdn.jsdelivr.net/gh/Cognifair/bio-exam-index@v1/points.json` |
| 元信息（章清单、题量、版本） | `https://cdn.jsdelivr.net/gh/Cognifair/bio-exam-index@v1/index.json` |
| 逐题分片（题干/选项/答案） | `https://cdn.jsdelivr.net/gh/Cognifair/bio-exam-index@v1/chapters/<文件名>` |
| 备用线路（GitHub raw） | `https://raw.githubusercontent.com/Cognifair/bio-exam-index/v1/points.json` |

## 引用语法

```
[Card#ID/<root_id>#<显示文字>]
```

- 只能用 `root_id`（`mkjr_` 开头），**不要用 card_id**（`mkjc_` 开头，会失效）；
- ID 大小写敏感，一个字符都不能改；
- 一条引用最多用短横线串 5 个 root_id：`[Card#ID/a-b-c#文字]`；
- 显示文字建议写「年份省份·考点」，学习者点开就知道是哪道题。

## 用法（三步）

1. 读 `points.json`，用你要讲的考点匹配 `point` 或 `keywords`；
2. 取该考点的 `root_ids`，最多挑 3 个；需要看题干时按 `chapter_file` 读对应分片；
3. 在你的卡片正文里写引用。

给 Agent 的完整提示词见 [`prompt.md`](prompt.md)，机器入口见 [`llms.txt`](llms.txt)。

## 字段

| 字段 | 含义 |
|---|---|
| `root_id` | 卡片引用 ID |
| `chapter` | 人教版（2019）章节 |
| `points` | 该题考查的考点（1–2 个） |
| `province` / `year` | 卷别 |
| `type` | 单选 / 多选 |
| `answer` | 正确答案字母 |
| `stem` / `options` | 题干与选项，`[图]` 表示原题此处有图 |
| `has_media` | 原题带图或表格，纯文字会缺条件 |
| `ref` | 可直接粘贴的引用片段 |

## 边界

- 索引只含题干、选项、答案、考点；**不含解析与插图**，解析在牌组里，点引用过去看。
- 真题文字版权归原命题方，本索引仅用于题目定位与学习检索。
- `root_id` 稳定：改卡不影响，删卡会使引用失效。
- 版本：当前 `v1`；更新见 [CHANGELOG.md](CHANGELOG.md)。
