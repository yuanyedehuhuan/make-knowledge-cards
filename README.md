# make-knowledge-cards

把一篇文章转换成 **5–8 张知识卡片**的 Agent Skill。每张卡片只讲一个知识点，包含标题、核心知识、简明解释、例子或自测问题，适合复习、转述和快速回顾。

## 特点

- **只抽取真正重要的知识**：文章标题、过渡句、纯轶事不会变成卡片
- **一卡一知识点**：重复内容合并；紧耦合的选项集合成一张卡
- **忠于原文**：不补充原文没有的事实、数据和例子，不纠正原文立场
- **不凑数量**：原文信息不足时允许少于 5 张，并在卡组开头说明
- **跟随原文语言**：中文文章 → 中文卡片内容；结构标签（`Card N`、字段名）固定为英文，保持 schema 稳定

## 支持的输入

| 输入方式 | 是否支持 |
| --- | --- |
| 对话中直接粘贴的文章 | ✅ |
| 本地 `.md` / `.txt` 文件 | ✅ |
| 网页 URL / 网页抓取 | ❌ |
| PDF、图片、Office 文档 | ❌ |
| Anki 牌组导出 / 图形界面 | ❌ |

## 安装

将 `skills/make-knowledge-cards` 目录复制到 Agent 读取 Skill 的目录即可：

```text
make-knowledge-cards/
├── SKILL.md
└── agents/
    └── openai.yaml
```

纯指令型 Skill，无第三方依赖、无脚本，开箱即用。

## 使用方法

直接提出需求即可，Skill 会根据描述自动触发：

- "把这篇文章做成知识卡片"（粘贴正文）
- "读一下 notes/xxx.md，帮我出一套复习卡片"
- "Turn this essay into 5-8 knowledge cards"
- "这篇教程帮我提炼成自测卡，要 JSON"

输出位置规则：

- **文件输入**：默认在源文件旁生成 `<源文件名>-cards.md`；显式指定路径时以指定为准
- **粘贴文本**：默认直接在对话中返回；需要存文件时说明路径即可

## 输入输出示例

输入一篇讲解费曼学习法的中文文章，输出：

```markdown
# Knowledge Cards: 费曼学习法：用"教"来逼自己"懂"

Source: sample-feynman.md · 5 cards

## Card 1: 知道名字不等于理解

- **Core knowledge:** 记住一个事物的术语或名称，与真正理解这个事物本身是两种完全不同的状态。
- **Explanation:** 费曼父亲用"知道鸟在各种语言里的名字，却对鸟本身一无所知"的例子说明……
- **Example / Self-test:** 自测：能流利说出一个术语却无法解释它，处在哪一层状态？

## Card 2: 用大白话讲解是理解的检验
...
```

信息不足的短文章会如实减少卡片数（自测题不附答案，答案即本卡的 Core knowledge）。

需要机器可读输出时，明确要求 JSON：

```json
{
  "source": "sample.md",
  "card_count": 2,
  "cards": [
    {
      "title": "...",
      "core_knowledge": "...",
      "explanation": "...",
      "example_or_self_test": "..."
    }
  ]
}
```

## 卡片结构

| 字段 | 内容 |
| --- | --- |
| **Title** | 知识点名称（英文 2–8 词 / 中文约 4–16 字），不是文章标题 |
| **Core knowledge** | 一两句可以直接记住的断言 |
| **Explanation** | 2–4 句，讲清含义、机制或重要性 |
| **Example / Self-test** | 一个取自原文的例子（优先），或一道开放式自测题（不附答案） |

## 校验

使用 OpenAI 官方 [skill-creator](https://github.com/openai/skills/tree/main/skills/.system/skill-creator) 中的校验脚本：

```bash
python3 quick_validate.py skills/make-knowledge-cards
```

检查 YAML frontmatter、必填字段、命名规范等。

## 测试

`tests/` 目录包含不同类型的测试文章及对应输出：中文方法论、英文观点文、技术教程、超短边界用例、中文专业文、英文粘贴转 JSON、URL 拒绝行为。

## 许可证

[MIT](LICENSE)
