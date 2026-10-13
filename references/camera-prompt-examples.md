# 摄影镜头参数验证 Prompt 集

> 用于验证 `SKILL.md` 中的“摄影镜头、机位与视角控制”是否被目标图像平台正确理解。每个案例都使用同一角色锚点，以隔离镜头、机位、焦段、滚转和相机距离带来的视觉变化。

## 使用方法

1. 使用同一个目标图像平台、同一画幅和同一角色参考图运行全部案例。
2. 平台支持时，为同一轮测试使用相同种子、相同风格预设和相同生成参数；若不支持，不要把单张结果视为绝对验证，应比较 2–4 次结果的共同趋势。
3. 不要在首次测试时改写镜头参数。先检查模型是否表现出目标机位、焦段和空间关系，再调整风格、细节或负面提示词。
4. 先运行 `T01` 作为角色基准。后续案例应将其作为共同角色参考；如果平台没有参考图功能，重复使用“固定角色锚点”。
5. 验收时优先看相机高度、俯仰、主体朝向、空间透视和滚转；不要仅凭“画面好不好看”判断参数是否生效。

## 共享测试条件

### 固定角色锚点

```text
A 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, and a calm angular face. She wears a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist.
```

### 固定视觉风格

```text
semi-realistic cinematic science-fiction concept art, restrained teal-and-amber palette, natural skin texture, believable fabric and wet-metal materials, clear facial identity, no text or watermark
```

### 统一测试建议

- 使用竖版 `4:5`；除非案例明确要求，角色脸部、发型、服装、主色和通信器均不可改变。
- 每次只修改案例中列明的镜头参数和最少量的叙事变量。
- 如平台支持排除指令，可复用：

```text
Avoid: identity drift, altered costume design, distorted hands, floating feet, inconsistent lighting direction, excessive props, unrelated decorative objects, unreadable focal hierarchy.
```

## T01：基准角色——眼平三分之四肖像

**验证目标**：验证稳定展示的默认参数：眼平、Pitch `0–5°`、Yaw `30–45°`、Roll `0°`、50–85mm。

| 参数 | 本例设定 |
|---|---|
| 景别 | 中全身 |
| 相机高度 | 眼平 `1.0H` |
| Pitch | 向上约 `3°` |
| Yaw | 角色三分之四朝向，约 `35°` |
| Roll | `0°` |
| 焦段 | 50mm |
| 场景预算 | 主体 + 1 个锚点（窗） |

```text
A clean cinematic character reference of a 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, and a calm angular face. She wears a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist.

Medium-full body framing, eye-level camera at the character's eye height, upward tilt around 3 degrees, three-quarter view at approximately 35 degrees, camera roll 0 degrees, 50mm lens. She stands in a relaxed neutral pose beside one tall transit-station window, looking slightly past the camera. The window is the only narrative anchor; keep the background simple and softly out of focus.

Soft neutral daylight from the left window, gentle contact shadow under the boots, subtle bounced cool light from the floor. Semi-realistic cinematic science-fiction concept art, restrained teal-and-amber palette, natural skin texture, believable fabric and wet-metal materials, clear facial identity, no text or watermark.
```

**预期画面信号**：五官和服装比例稳定；背景不会明显拉伸；角色既不被俯视也不显得压迫观众；画面水平线稳定。

**不通过时优先检查**：若脸部变形或环境过宽，确认平台是否将 50mm 误解为广角；移除额外的“wide-angle”“dynamic perspective”等冲突词。

---

## T02：力量登场——低机位广角

**验证目标**：验证低机位、向上 Pitch `20°`、24mm 广角与轻度透视夸张如何共同建立力量感。

| 参数 | 本例设定 |
|---|---|
| 景别 | 全身中景 |
| 相机高度 | 膝部以下，约 `0.5H` |
| Pitch | 向上约 `20°` |
| Yaw | 三分之四正面，约 `20°` |
| Roll | `0°` |
| 焦段 | 24mm |
| 场景预算 | 主体 + 1 个锚点（站台边缘）+ 1 组支持元素（失焦铁轨） |

```text
A cinematic character key visual of a 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, and a calm angular face. She wears a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist.

Full-body medium-wide shot, camera positioned at knee level, upward tilt around 20 degrees, three-quarter front view at approximately 20 degrees, camera roll 0 degrees, 24mm wide-angle lens. She stands with both boots firmly planted on the edge of a rain-wet elevated transit platform, shoulders squared, one hand gripping the messenger-bag strap. The platform edge is the narrative anchor; only blurred rails recede behind her as the support group.

Cool cyan platform light from the left, a restrained warm rim light from a distant arriving train behind her, wet-pavement reflections on the boots and lower jacket, physically consistent contact shadows. Semi-realistic cinematic science-fiction concept art, restrained teal-and-amber palette, natural skin texture, believable fabric and wet-metal materials, clear facial identity, no text or watermark.
```

**预期画面信号**：角色略高大、有向上延展感；站台边缘和铁轨有近大远小；脸部仍可辨识，但不应像 14mm 近距离那样显著拉伸。

**不通过时优先检查**：若没有力量感，先缩短相机距离或强化 `camera positioned at knee level`，而不是继续提高焦段；若脸部和脚部过度失真，改为 28mm 或略拉远相机。

---

## T03：克制孤独——高机位与留白

**验证目标**：验证高于主体的相机、向下 Pitch `25°`、50mm 与负空间如何传达脆弱和孤独，而不依赖堆叠悲伤道具。

| 参数 | 本例设定 |
|---|---|
| 景别 | 中广景 |
| 相机高度 | 高于头部，约 `1.3H` |
| Pitch | 向下约 `25°` |
| Yaw | 接近正面，约 `10°` |
| Roll | `0°` |
| 焦段 | 50mm |
| 场景预算 | 主体 + 1 个锚点（通信器）+ 1 组支持元素（失焦铁轨） |

```text
A cinematic science-fiction scene featuring a 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, and a calm angular face. She wears a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist.

Medium-wide shot, camera positioned above the character's head, looking down at approximately 25 degrees, near-front view at approximately 10 degrees, camera roll 0 degrees, 50mm lens. She sits alone on a rain-wet transit-platform bench, eyes lowered to the communication watch in her hands. The watch is the narrative anchor; blurred rails occupy only the far background. Place the character in the lower-right third and preserve expansive clean negative space above and to the left.

Soft cool overcast light, a faint reflected cyan glow from the wet floor, subtle contact shadow under the boots and bench. Semi-realistic cinematic science-fiction concept art, restrained teal-and-amber palette, natural skin texture, believable fabric and wet-metal materials, clear facial identity, no text or watermark.
```

**预期画面信号**：观众从略高位置观察角色；角色在画面中显得较小、被留白包围；50mm 保持空间不过度夸张，孤独主要由角度和留白形成。

**不通过时优先检查**：若像普通平视坐姿，强化 `camera positioned above the character's head` 和 `looking down at approximately 25 degrees`；若背景喧闹，删去环境物件而不是再增加雾、雨、人群或广告屏。

---

## T04：关系叙事——肩后镜头

**验证目标**：验证肩后前景、35mm 和中性水平线如何建立“观察 / 对话 / 窥视”的角色关系。

| 参数 | 本例设定 |
|---|---|
| 景别 | 肩后中景 |
| 相机高度 | 前景人物肩部高度 |
| Pitch | `0–5°` |
| Yaw | 对准被观察角色 |
| Roll | `0°` |
| 焦段 | 35mm |
| 场景预算 | 前景肩部 + 主体 + 1 个锚点（车窗） |

```text
Over-the-shoulder cinematic shot in a futuristic transit station. In the softly blurred foreground, show only the right shoulder and partial profile of an older station attendant. In the midground, show a 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, and a calm angular face. She wears a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist.

Shoulder-height camera behind the attendant, pitch approximately 3 degrees upward, aimed at the courier, camera roll 0 degrees, 35mm lens. The courier stands beside one train window and looks outside rather than toward the attendant. The train window is the narrative anchor; keep all other station details subdued and out of focus.

Cool interior light from above with a warm passing-train reflection on the courier's cheek and jacket sleeve, shallow but readable depth of field, clear focus on the courier's eyes. Semi-realistic cinematic science-fiction concept art, restrained teal-and-amber palette, natural skin texture, believable fabric and wet-metal materials, clear facial identity, no text or watermark.
```

**预期画面信号**：前景肩部应形成明确的观察者位置但不遮挡主体脸部；35mm 同时保留关系空间和人物可读性；观众会感觉自己站在前景人物身后。

**不通过时优先检查**：若前景人物过大或主体被遮住，降低前景肩部占比；若画面变成普通双人并排肖像，强调 `behind the attendant` 和 `foreground shoulder softly blurred`。

---

## T05：危险临界——荷兰角

**验证目标**：验证 Roll `12°` 能产生失衡感，而不破坏人物、重力和场景光线的基本逻辑。

| 参数 | 本例设定 |
|---|---|
| 景别 | 半身中景 |
| 相机高度 | 眼平略低 |
| Pitch | 向上约 `8°` |
| Yaw | 三分之四，约 `30°` |
| Roll | `12°` |
| 焦段 | 35mm |
| 场景预算 | 主体 + 1 个锚点（破裂的通信器）+ 1 组支持元素（倾斜的失焦灯带） |

```text
A tense cinematic moment featuring a 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, and a calm angular face. She wears a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist.

Medium shot, camera slightly below eye level, upward tilt around 8 degrees, three-quarter view at approximately 30 degrees, camera roll 12 degrees to create a deliberate Dutch angle, 35mm lens. She holds a cracked communication watch close to her chest and turns toward an unseen sound. The damaged watch is the narrative anchor; include only one blurred diagonal strip of station lighting as the support group.

Hard cool light from the left, a restrained amber edge light from behind, consistent cast shadows and grounded posture despite the tilted frame. Semi-realistic cinematic science-fiction concept art, restrained teal-and-amber palette, natural skin texture, believable fabric and wet-metal materials, clear facial identity, no text or watermark.
```

**预期画面信号**：水平线与灯带明显倾斜，但角色不会像失重或倒下；Roll 制造心理失衡，主体仍是清晰焦点。

**不通过时优先检查**：若“危险感”不足，先确认 Roll 是否达到可见的 `8–15°`；若画面混乱，减少道具和前景，不要继续增加倾斜元素或使用鱼眼。

---

## T06：动作冲击——透视缩短

**验证目标**：验证“低机位 + 18mm + 镜头靠近动作前端”必须同时出现，才能得到可读的透视缩短与速度感。

| 参数 | 本例设定 |
|---|---|
| 景别 | 动作中近景 |
| 相机高度 | 接近地面 |
| Pitch | 向上约 `15°` |
| Yaw | 三分之四正面，约 `15°` |
| Roll | `0–4°` |
| 焦段 | 18mm |
| 场景预算 | 主体 + 1 个锚点（飞出的通信器）+ 1 组支持元素（失焦站台边缘） |

```text
A dynamic cinematic action shot of a 27-year-old East Asian female courier with a short black bob, a small silver hair clip on the left side, amber eyes, and a calm angular face. She wears a dark navy cropped courier jacket with reflective seams, an orange messenger bag, black cargo trousers, worn boots, and a vintage communication watch on her left wrist.

Close action shot from a ground-level camera, upward tilt around 15 degrees, three-quarter front view at approximately 15 degrees, camera roll 3 degrees, 18mm wide-angle lens. The camera is very close to her leading right hand as she lunges forward to catch a falling communication watch; strong foreshortening makes the hand and watch appear larger while her face remains visible behind them. The falling watch is the narrative anchor; show only a blurred platform edge receding into the background.

Directional cool light from the left, fast-moving coat and hair, believable forward momentum, dust and tiny water droplets kicked up near the boots, consistent contact and motion blur. Semi-realistic cinematic science-fiction concept art, restrained teal-and-amber palette, natural skin texture, believable fabric and wet-metal materials, clear facial identity, no text or watermark.
```

**预期画面信号**：前手和通信器应明显大于脸部，平台边缘向远处收缩，动作方向清楚；这来自相机距离和广角的组合，而不是单独的 `foreshortening` 词。

**不通过时优先检查**：若没有近大远小，强化 `camera is very close to her leading right hand`；若出现过度手部畸变，改为 24mm、稍拉远相机，或使用姿态控制后局部修复。

## 结果记录与对比

每次测试后记录下表。不要将一次偶然成功或失败当作模型对参数的最终能力结论。

| 案例 | 应验证的主信号 | 是否通过 | 失败表现 | 下一步最小调整 |
|---|---|---|---|---|
| T01 | 眼平、三分之四、50mm 的稳定身份展示 |  |  |  |
| T02 | 低机位 20° 上仰、24mm 的力量与空间张力 |  |  |  |
| T03 | 高机位 25° 下俯、50mm 与留白的孤独感 |  |  |  |
| T04 | 肩后前景与 35mm 的关系空间 |  |  |  |
| T05 | Roll 12° 的可见失衡且人物不失重 |  |  |  |
| T06 | 18mm 近距离镜头的透视缩短和动作方向 |  |  |  |

## 结论判定

- 若 `T01` 不稳定，优先解决角色参考、身份锚点和基准图问题，再测试复杂机位。
- 若机位方向正确但焦段差异不明显，说明平台可能弱化了物理焦段指令；保留焦段词，同时强化相机距离和透视结果描述。
- 若荷兰角、俯仰或鸟瞰经常失效，优先加强明确的角度语言，例如 `camera roll 12 degrees`、`looking down at approximately 25 degrees`，不要叠加更多风格词。
- 若动作透视成功但肢体崩坏，先减少动作复杂度或使用结构控制，再进行局部编辑；不要退回到平视中景而丢失目标镜头语言。
- 将稳定通过的案例作为平台特定的可复用预设；记录模型、画幅、参考图、种子、控制工具和调整结果。
