# Resume Optimizer

一个面向求职者的简历优化 Skill。

它专注做一件事：把普通、松散、偏职责型的简历，审计并重写成更有说服力的求职材料，方便你单独开源、维护和分发。

## 这个项目解决什么问题

- 帮你找出简历里最致命的问题，而不是泛泛而谈
- 把“负责了什么”改成“做成了什么”
- 主动挖出量化结果、业务价值和实际交付产物
- 根据目标 JD 调整关键词、项目重点和职业叙事
- 生成一版可继续打磨的改写稿
- 在用户需要时生成一份压缩后的一页简历
- 识别外包经历、玩具项目、量化缺失、技术表述失真等常见风险

## 项目结构

```text
resume-optimizer/
├── README.md
├── LICENSE
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── audit-checklist.md
    ├── narrative-tools.md
    ├── one-page-resume.md
    └── red-flags.md
```

## 安装

### Codex

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/wyh0626/resume-optimizer.git ~/.codex/skills/resume-optimizer
```

### Claude Code

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/wyh0626/resume-optimizer.git ~/.claude/skills/resume-optimizer
```

### 手动安装

把整个目录复制到你的 skills 目录中，至少保留：

- `SKILL.md`
- `references/`
- `agents/openai.yaml`

## 使用方式

可以显式调用：

```text
Use $resume-optimizer to audit my resume for a senior backend role.
```

也可以自然语言触发：

```text
请帮我审计这份简历，并按高级后端工程师 JD 优化重点。
```

```text
把我这段项目经历从职责描述改成成就描述，不要编造数据。
```

```text
请把这份两页简历压缩成一页，并优先保留最能证明我价值的成果。
```

## 输出风格

这个 skill 默认会给出：

1. 30 秒结论
2. 关键问题清单
3. 价值提炼与量化补强点
4. 修改策略
5. 改写后的简历片段或整版草稿
6. 是否需要一页版的确认
7. 下一步行动清单

## 适合什么场景

- 社招技术岗简历优化
- 校招项目描述提炼
- 投递前按 JD 定制简历
- 把两页以上的旧简历压缩成一页版
- 面试前自查简历漏洞
- 外包经历、转岗经历、空窗期的叙事修复
