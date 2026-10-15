# 角色视觉导演与一致性控制

> 为 AI 角色图提供“先锁定身份，再导演画面，最后最小化修复”的轻量工作流。

[查看最新发布版](https://github.com/tingting-CY/character-visual-direction-skill/releases/latest) · [查看核心规则](./SKILL.md)

## 适合解决什么问题

- 同一个角色在多张图里换脸、换服装或失去辨识度；
- 人物平视站桩、无动势、表情模板化；
- 背景元素过多，画面没有视觉焦点；
- 人物像贴在背景上，光影、投影和反射不一致；
- 皮肤、衣服、金属都像同一种塑料；
- 想做非均分四宫格、镜头递进或主角越框的漫画页。

## 最小运行包

实际运行时只需要以下三个文本文件：

```text
SKILL.md
references/realism-and-coherence-control.md
references/comic-panel-layouts.md
```

本 README 仅供 GitHub 浏览和人工快速上手，**不属于运行规则**。仓库通过 `.gitattributes` 将 README 排除在 GitHub 自动生成的 Source code ZIP/TAR.GZ 之外，因此 Release 的源码归档仍只包含上述三个运行文件。

## 快速使用

1. 从 [Releases](https://github.com/tingting-CY/character-visual-direction-skill/releases/latest) 下载最新版本的 **Source code (zip)**，或克隆仓库。
2. 将解压后的目录作为 Skill 导入支持 `SKILL.md` 的智能体环境；先让智能体读取 `SKILL.md`。
3. 提供角色锚点、任务类型和本轮想变化的内容。多图任务先准备一张确认过的角色基准图。
4. 仅在碰到专项问题时读取对应的 `references/` 文件，而不是每次加载全部内容。

可直接这样发起任务：

```text
请使用“角色视觉导演与一致性控制”Skill，基于以下信息输出创作方案、主提示词和验收重点：

任务类型：单张主视觉 / 多图变体 / 九宫格 / 四格漫画 / 局部修复
角色锚点：
绝不可变化项：
本轮只变化：
叙事时刻与目标情绪：
目标平台与画幅：
```

## 按需读取的参考模块

| 遇到的情况 | 读取文件 |
|---|---|
| “很 AI”、贴图感、假笑、塑料感、动作悬浮或无意义居中 | [`references/realism-and-coherence-control.md`](./references/realism-and-coherence-control.md) |
| 非 2×2 四宫格、漫画镜头递进、主角破格 / 越框 | [`references/comic-panel-layouts.md`](./references/comic-panel-layouts.md) |

## 使用原则

- 先确定角色身份，再扩展场景、动作和镜头。
- 每张图只确定一个主要镜头、一个动作意图、一个主光源、一个视觉焦点和一个叙事锚点。
- 默认使用“1 个角色 + 1 个叙事锚点 + 0–2 组支持元素”，用光线、天气和景深营造氛围，而不是堆叠道具。
- 每轮只改变 1–2 个关键变量；先检查身份、姿态和空间，再处理材质或微瑕疵。

## 版本说明

- [`v1.0.0`](https://github.com/tingting-CY/character-visual-direction-skill/releases/tag/v1.0.0)：首个最小可用版本。
- `v1.0.1`：补充仓库层的精简使用指南；不改变 Skill 的执行规则与运行文件。

## 边界

本 Skill 用于角色主体的图像生成和编辑规划，不适用于纯风景、产品电商图、普通图片格式处理或无关文案。它提升可控性，但不承诺仅凭文本实现绝对的人物一致性或零错误人体。
