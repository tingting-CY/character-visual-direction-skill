# 画面自洽与反塑料控制

> 仅在角色图出现“贴图感、假笑、塑料感、木偶动作、居中像证件照”时读取。目标是定位一个可见根因并做最小修复，不是继续叠加泛化风格词。

## 先判症状，再修根因

| 症状 | 首查根因 | 最小修复 |
|---|---|---|
| 人物贴在背景上 | 主光、投影、环境反射或接触阴影不一致 | 修人物边缘、脚部或接触点；不要加背景道具 |
| 标准假笑 / 精修假面 | 表情没有事件和身体反馈；皮肤只被描述为平滑漂亮 | 指定触发事件、非对称微动作和适度表面细节 |
| 动作像悬浮木偶 | 缺支撑点、重心、力方向、惯性或环境反馈 | 先锁支撑脚/手和重心，再补衣发与地面反应 |
| 所有表面像塑料 | 未区分粗糙度、反射、高光、纹理或透光 | 仅选择 2–4 种关键材质分别描述 |
| 居中像证件照 | 未指定主体位置、视线、负空间和对重元素 | 定义角色位置和视线落点，再压低背景 |

修复顺序：**身份 → 姿态/重心 → 光线/空间 → 构图 → 材质 → 微瑕疵**。

## 五项契约

在写 Prompt 前，先各用一句具体的话回答：

```text
光线：主光来自哪里、什么颜色、影子落向哪里、环境反射在哪里？
表情：发生了什么，角色的眼、口、下颌、肩手如何共同表现？
动力：重量由哪里支撑，身体向哪里发力，衣发/道具向哪里拖尾？
材质：哪 2–4 种材质最关键，它们怎样吸光、反光或透光？
构图：角色在哪里、看向哪里、留白和次要信息放在哪里？
```

任一项答不出时，先简化场景；不要添加 `cinematic`、`masterpiece`、`highly detailed` 或更多道具。

## 可直接复用的约束

### 光线与空间

```text
[主光] from [方向] with [色温]; shadows fall [方向]. A weak [环境颜色] bounce appears on [区域], with contact occlusion at [脚底/手握处/衣领/坐面]. No unrelated studio fill light.
```

检查：脚是否踩地、手是否触物、人物/道具/环境的影子是否同向、环境色是否轻微影响角色边缘。

### 表情

```text
After [触发事件], [主情绪] appears through [眼/口/下颌的非对称微动作] and [肩、手或重心的身体反馈], avoiding a default posed smile.
```

示例：`eyes lowered, lips gently pressed, shoulders drawn inward, fingers tightening around the letter`。写实角色可使用自然皮肤纹理；动画、漫画、儿童或时尚项目不默认添加毛孔、皱纹、脏污。

### 动势

```text
[动作] with weight supported by [支撑点]. The center of mass shifts [方向]; [惯性] pulls the torso [方向]. [地面/道具] responds with [水花/尘/拉力/压痕], and [衣发/包/围巾] trails [方向].
```

高风险跑跳、搏斗或复杂手势：优先姿态参考/控制；不支持时改为“起跑”“落地”“转身”等单一瞬间。

### 材质

```text
Contrast [柔软材质的织纹/宽高光] with [硬质材质的反射/锐高光/磨损], allowing [环境颜色] to appear differently on each surface.
```

材质差异由受光行为而非物体名称体现：布料高光宽而软，金属高光窄而锐，玻璃/湿面有透光或倒影，皮革有粒面与边缘磨损。

### 构图

```text
Character on the [左/右] third, looking toward [留白/锚点/画外威胁]. Preserve [方向] clean negative space; use one low-detail [对重元素] and sharp focus on [脸/动作].
```

不要为避免居中而随机裁切。偏心、留白、荷兰角或虫眼必须服务孤独、行动、危机或压迫等明确事件。

## 局部修复模板

```text
Preserve the original character identity, pose, camera angle, clothing, lighting direction, color grading, and background. Repair only [区域]: [具体问题]. Match [主光来源/方向], [阴影/反射], [相邻材质], and [透视/遮挡关系]. Do not alter [保持区域].
```

## 五问验收

1. 能否从人物亮面、投影和反射指出主光来源？
2. 不看嘴角，能否从视线、下颌、肩手和动作读出情绪？
3. 能否指出支撑点、重心方向及一个环境或衣物反馈？
4. 关键表面是否至少呈现两种不同的受光行为？
5. 若把角色移回中心，画面是否会失去当前的叙事意义？

任一项不通过，只修复对应项。平台支持局部编辑时，优先局部修复；支持参考图时，用参考图锁定身份；支持姿态/深度控制时，先解决骨架和空间，再处理材质。
