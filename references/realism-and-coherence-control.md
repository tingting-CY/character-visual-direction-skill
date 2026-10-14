# 画面自洽与反塑料控制

> 用于诊断并修复角色图“很 AI”“像素材拼贴”“表情假”“动作僵”“材质同质”或“居中像证件照”等问题。将抽象评价转为可见、可检验的画面约束。适用于写实、半写实和电影感项目；漫画、动画或高度风格化项目应保留其风格化表达，但同样需要光线、动作、材质和构图的内部自洽。

## 核心方法：先判根因，再做最小修复

不要在失败图上持续叠加 `cinematic`、`highly detailed`、`masterpiece`、`realistic` 等泛化词。先识别最先破坏可信度的一个根因，再只修改对应模块。

| 症状 | 首要检查根因 | 最小修复方向 |
|---|---|---|
| 人物像贴在背景上 | 主光、投影、环境反射或接触阴影不一致 | 先重写光线契约；只重绘人物边缘、脚部或接触处 |
| 人物像精修塑料人 | 表情没有行为动机；皮肤/材质只有“平滑漂亮”描述 | 用可见的非对称表情和合适的微纹理替代泛化美化词 |
| 动作像悬浮木偶 | 缺少支撑点、重心、力量方向、惯性或环境反馈 | 先锁动作骨架和支撑脚，再添加衣发、尘水和道具反应 |
| 皮肤、衣服、金属像同一材质 | 未描述不同表面的粗糙度、反射、边缘磨损和受光差异 | 只挑 2–4 种关键材质分别描述 |
| 角色像证件照般居中 | 未指定主体位置、视线方向、负空间、裁切或背景对位 | 先定义主体位置与视线落点，再调整背景和焦点 |

修复优先级：**身份 → 姿态/重心 → 光线/空间 → 构图 → 材质 → 微瑕疵**。脸或服装已经漂移时，不要先修胶片颗粒、灰尘或皮肤纹理。

## 写实与自洽契约

在场景化图像 Prompt 前，先在内部完成以下五项契约。只将与任务有关的内容写进 Prompt。

```text
光线契约：主光来自哪里？色温、方向、影子和反射如何表现？
表情契约：角色因何产生情绪？眼、口、下颌、肩和手如何共同表现？
动力契约：力量从哪里来？重心在哪只脚或接触点？衣发和物件往哪里运动？
材质契约：哪 2–4 种材质最关键？各自如何吸光、反光、磨损或透光？
构图契约：角色位于哪里？看向哪里？空白和次要信息位于哪里？
```

任一契约无法用一句具体的话描述时，先简化场景，而不是继续增加道具或风格词。

---

## 1. 光线逻辑与空间统一

### 光线契约

每张场景图至少指定以下五项；不必机械写出术语，但必须让画面能推导出结果。

| 项目 | 必须明确的内容 | 可观察结果 |
|---|---|---|
| 主光 | 来源、方向、色温、相对强度 | 脸部亮暗面、衣物亮边、地面明暗关系一致 |
| 阴影 | 落向、软硬、长度 | 人、道具与环境影子方向一致 |
| 环境反射 | 周围主要表面的颜色与方向 | 湿地、金属、玻璃、下颌或衣摆出现合理弱色光 |
| 接触遮蔽 | 脚底、衣领、手握物、坐面等接触位置 | 物体不悬浮，重叠处有自然暗部 |
| 补光边界 | 是否存在补光；若存在，来源是什么 | 不出现没有来源的影棚平光 |

### Prompt 写法

不要单独写 `consistent lighting` 或 `perfectly integrated` 并期待模型自行推理。用可见因果描述：

```text
Single cool cyan key light from the high-left station sign; the character's cast shadow falls softly to the lower right. A weak amber bounce from the wet platform colors the lower jacket hem and boots. Subtle ambient occlusion at the boot soles, collar, and hand gripping the rail. No neutral studio fill light.
```

| 场景 | 有效写法 | 避免 |
|---|---|---|
| 窗边日光 | `directional daylight from the left window, long soft-edged shadows cast to the right` | 只写 `soft cinematic window light` |
| 霓虹雨夜 | `cyan storefront key light from the left, restrained magenta rim light from behind, reflections on wet pavement and lower coat` | 同时要求红蓝绿紫四种无来源光 |
| 阴天外景 | `soft overcast skylight, muted palette, short diffuse shadows, cool bounce from wet concrete` | 仍给人物硬质舞台聚光 |
| 烛火/灯笼 | `warm low key light from the candle below the face, rapid falloff into shadow, faint warm reflection on the metal clasp` | 角色背面比烛火正面更亮 |

### 光影割裂诊断

1. **先看接触点**：脚是否踩在地面上；手是否真正握住物体；坐姿是否压出坐面阴影。
2. **再看影子方向**：人物、道具和背景的影子是否指向同一方向。
3. **再看环境色**：雨夜蓝色、木地板暖色、沙地黄反光是否轻微影响角色下半身和阴影边缘。
4. **最后看边缘**：角色轮廓是否有不合来源的白边、影棚轮廓光或抠图感。

局部修复模板：

```text
Preserve the character identity, pose, camera angle, and scene layout. Repair only [区域]. Match the existing [主光来源] from [方向], make the cast shadow fall [方向], add a subtle [环境颜色] bounce on [区域], and add contact occlusion at [接触点]. Do not add studio lighting or change the background.
```

---

## 2. 表情真实性与反“标准假笑”

表情不能只由嘴角决定。用“**触发事件 + 主情绪 + 不对称微动作 + 身体反馈**”描述。角色在高压、悲伤、警觉或疲惫情境中，不默认添加微笑。

| 主情绪 | 可见组合 | 可用表达 |
|---|---|---|
| 克制悲伤 | 视线压低、嘴唇轻压、肩略收、手握紧物件 | `eyes lowered, lips gently pressed, shoulders drawn inward, fingers tightening around the letter` |
| 警觉 | 眼神偏向威胁、下颌微紧、重心准备移动 | `glancing toward an off-frame sound, jaw subtly tense, weight shifted toward an exit` |
| 决心 | 视线稳定、下颌收紧、站姿扎根 | `steady gaze, jaw set, grounded stance, controlled determination` |
| 真正快乐 | 眼周参与、嘴角不完全对称、肩颈松开 | `genuine asymmetrical laugh, eyes slightly squinting, relaxed shoulders` |
| 愤怒/反抗 | 身体前倾、眉眼与下颌共同紧张、动作有指向 | `brows drawn, jaw tense, leaning forward into the wind, defiant glare` |
| 疲惫 | 眼睑微垂、呼吸感、姿态下沉 | `heavy eyelids, a quiet exhale, posture sinking slightly after exertion` |

### Prompt 约束

```text
[触发事件] causes [主情绪]. Show it through [眼部/嘴部/下颌的不对称微动作] and [肩、手或重心的身体反馈], avoiding a default posed smile.
```

示例：

```text
After reading the emergency signal, she does not smile: her eyes widen slightly toward the screen, her lower lip tightens, and one shoulder rises as her fingers close around the watch.
```

写实人物可加入 `natural skin texture`、`subtle pores`、`fine expression lines` 等微观信息；动画、漫画、时尚摄影或儿童角色不默认使用雀斑、皱纹、素颜、脏污等写实化词，而应改用与画风一致的线条、色块或表情设计。

---

## 3. 动势、重心与物理反馈

将动作拆成五个可回答的问题。缺一不可时，优先删除高风险动作，而不是用 `dynamic pose` 覆盖问题。

| 问题 | 应明确的内容 | Prompt 表达 |
|---|---|---|
| 支撑点 | 哪只脚、手、座面或物件承受重量？ | `weight planted through the left boot`, `one hand gripping the rail` |
| 重心 | 身体重量相对支撑点在哪里？ | `weight shifted forward over the bent front knee` |
| 主力方向 | 推、拉、跌落、转身或风力朝哪里？ | `momentum pulling the torso forward` |
| 反作用 | 地面、墙、道具或对手如何反馈？ | `water splashing outward from the planted boot` |
| 拖尾 | 头发、衣摆、围巾、包或尘水往哪里延迟？ | `coat hem and messenger bag trailing behind the turn` |

### 动作 Prompt 骨架

```text
[动作] with weight supported by [支撑点]. The center of mass shifts [方向] over [身体部位]; [主力/惯性] pulls the torso [方向]. [地面/道具] responds with [尘、水花、压痕、拉力或反冲]. [头发/衣摆/围巾/包] trails [相反或滞后方向].
```

### 示例

```text
She lunges forward with weight planted through the bent left boot. Her center of mass shifts over the front knee while momentum pulls her torso forward; water splashes outward from the sole, and the orange messenger bag and coat hem trail behind the turn.
```

### 高风险动作策略

| 风险 | 第一选择 | 第二选择 |
|---|---|---|
| 奔跑、跳跃、搏斗、舞蹈 | 姿态控制/参考图锁定骨架 | 简化为一个动作瞬间，例如“起跑”或“落地” |
| 复杂手势 | 先减少手指与道具交互 | 局部重绘手部，不重做整张图 |
| 失重感 | 加支撑点、接触阴影、地面反馈 | 改为靠墙、半蹲或单脚换重心的稳定动作 |
| 风太多、动势太乱 | 保留一个主力方向 | 删除多重风、飘带、粒子和同时运动的道具 |

---

## 4. 材质差异与反“3D 塑料味”

材质提示不应是物体名称清单。选择 2–4 种关键材料，并让它们在**粗糙度、反射清晰度、高光形状、边缘磨损、纹理尺度和透光性**上形成对比。

| 材质 | 观察重点 | 可用表达 |
|---|---|---|
| 皮肤 | 柔和过渡、细微不均、低强度环境反射 | `natural skin texture, subtle pores, soft light falloff, gentle subsurface warmth` |
| 棉/织物 | 织纹、褶皱、较宽的柔和高光 | `woven fabric texture, creased cotton, broad soft highlights` |
| 天鹅绒 | 方向性绒面、高光随纤维方向变化 | `soft velvet with directional sheen` |
| 皮革 | 粒面、边缘磨损、半哑光反射 | `rough leather grain, subtle scuffs, worn edges` |
| 拉丝金属 | 细长硬高光、环境色反射、磨损边缘 | `brushed steel, crisp specular highlights, faint cyan reflections` |
| 哑光陶瓷 | 扩散反射、几乎无镜面倒影 | `matte ceramic, soft diffuse reflections` |
| 玻璃/湿面 | 透光、边缘高光、折射或倒影 | `translucent glass, wet-surface reflections, condensation` |

### 材质 Prompt 骨架

```text
Contrast [柔软材质] with [粗糙或硬质材质]: [柔软材质的纹理/高光] against [硬质材质的反射/边缘/磨损]. Let the scene's [环境颜色] appear differently on each surface.
```

示例：

```text
Contrast the woven navy jacket's broad soft highlights with the messenger bag's rough leather grain and the communication watch's brushed metal, which carries a narrow cyan reflection from the station sign.
```

不要给每一个物体都增加纹理。将视觉注意力留给角色脸部、叙事锚点和 2–4 种关键材质；其余物体降为简化或失焦背景。

---

## 5. 破除居中惯性与安全区依赖

“角色不居中”不等于随机裁切。先指定角色位置、视线方向、负空间用途、背景对位和视觉对重，再决定是否裁切。

### 构图契约

| 项目 | 必须决定的问题 | 可用表达 |
|---|---|---|
| 主体位置 | 角色位于左/右/下三分区？面部在哪个视觉节点？ | `character positioned on the far left third` |
| 视线向量 | 角色看向何处？视线是否引向留白、道具或画外威胁？ | `looking into the empty space on the right` |
| 负空间 | 空白用于孤独、标题、威胁来源还是等待动作？ | `expansive clean negative space on the right` |
| 对重元素 | 哪一个低细节元素平衡人物重量？ | `one distant defocused station sign balancing the frame` |
| 边缘裁切 | 是否通过裁切制造紧迫、亲近或窥视？ | `intentional crop at the lower right edge` |
| 焦点层级 | 哪里最亮、最清晰、对比最大？ | `sharp focus on the eyes; subdued low-detail background` |

### 三种安全预设

| 目标 | 构图方案 | Prompt 示例 |
|---|---|---|
| 孤独 / 等待 | 角色放左或右三分位，看向大块留白 | `character on the left third, looking into wide empty space on the right` |
| 决定 / 出发 | 角色偏左或偏右，动作与视线指向画面内部 | `off-center framing, body and gaze directing toward the open path` |
| 危机 / 紧迫 | 角色接近边缘，可轻微裁切，威胁留在画外 | `extreme off-center composition, intentional edge crop, threat implied off-frame` |

荷兰角、虫眼和极端裁切都只能服务于一个明确事件。若只是为了避免居中而加入，会增加失控风险；优先使用三分法、视线方向、负空间和一个对重元素。

---

## 组合 Prompt：五项契约的标准版

仅在场景需要写实/半写实可信度时使用；根据画风删减不适用模块。

```text
A cinematic [style] scene of [角色锚点].

Composition: [主体位置], [视线方向], [负空间用途], [一个低细节对重元素].
Camera: [景别、机位、Pitch、Yaw、Roll、焦段和距离].
Action: [动作] with weight supported by [支撑点]; center of mass shifts [方向]; [衣发/道具] trails [方向]; [地面或道具反馈].
Expression: after [触发事件], [主情绪] shown through [不对称微动作] and [身体反馈], avoiding a default posed smile.
Scene: [一个叙事锚点] and [至多 0–2 组支持元素].
Lighting: [主光来源、方向、色温]; shadows fall [方向]; [弱环境反射]; ambient occlusion at [接触点]; no unrelated studio fill.
Materials: contrast [2–4 种关键材质及其粗糙度、纹理和反射差异].
[画风与色彩]. Clear focal hierarchy, no unrelated decorative objects.
```

## 质量验收：反“AI 感”五问

在提交前，对每项回答“是 / 否”。任何一项为“否”，只修复对应模块。

1. **光线**：能否指出画内的主光来源，并从人物亮面、阴影落向和环境反射中得到验证？
2. **表情**：不看嘴角，是否仍能从视线、下颌、肩手和动作看出角色的主情绪？
3. **动力**：能否指出身体的支撑点、重心方向与至少一个环境或衣物反馈？
4. **材质**：关键皮肤/布料/金属/玻璃之间是否至少有两种不同的受光行为？
5. **构图**：若把角色移回画面中心，画面是否会失去当前的留白、视线、紧迫或叙事意义？若不会，当前偏心缺乏目的。

## 平台能力分流

- **仅文本生成**：减少变量，优先锁定光线契约和构图契约；动作复杂时选择稳定瞬间。
- **支持参考图**：用角色基准图锁脸和服装，文本只控制本轮的光线、动作、材质或构图变量。
- **支持姿态/深度控制**：动作与透视问题优先结构控制；不要先用材质词掩盖骨架问题。
- **支持局部编辑**：修复光影、接触阴影、手部、表情和材质时，只处理最小区域并继承原图光线/构图。
- **支持负面提示词**：仅在平台支持时加入 `avoid default studio beauty lighting, default posed smile, centered passport-photo composition, floating feet, uniform plastic material`；不要用超长禁词替代正向画面设计。
