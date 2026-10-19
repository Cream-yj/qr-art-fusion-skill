# 整体生成与后端选择

## 先确定控制能力

整体融合式作品让连续画面承担二维码的明暗结构。原矩阵是目标；生成图的格点、轮廓和局部对比可能发生变化。保留原矩阵文件不等于最终像素仍精确保留该矩阵。

| 可用能力 | 合理用途 | 需要如实说明的范围 |
|---|---|---|
| 带 QR 条件控制的本地扩散后端 | 从原码的结构/明暗控制图生成整幅候选 | 实际 checkpoint、控制模型和运行参数 |
| 可用的整图生成/编辑工具 | 创建视觉候选、编辑人物或服装、生成局部修整 | 不将参考图引导描述为已执行 ControlNet；候选仍须验码 |
| 确定性图像合成工具 | 将已认可的细节或二维码原图放入指定版式 | 实际蒙版、缩放、偏移、裁切与改变范围 |

只整理 Skill、研究方法或写提示词时，不启动推理。已有认可的修好素材时，优先复用，不必为了转移到另一版式重新生成。

## 历史使用的本地配置

这是历史运行记录，不是安装脚本、通用最优参数或本次重新验证的配置。

| 角色 | 历史配置 |
|---|---|
| 基础管线 | Stable Diffusion 1.5 兼容的 Diffusers ControlNet 管线 |
| 主模型 | ReV Animated 1.2，文件 `rev_1.2.safetensors` |
| 二维码控制 | `monster-labs/control_v1p_sd15_qrcode_monster` 的 v2 权重 |
| 明暗控制 | `latentcat/control_v1p_sd15_brightness` |
| 采样器 | DPM-Solver++ SDE，使用 Karras sigmas |
| 一次山水候选的画布 / 步数 / CFG | 512×512 / 40 / 7 |
| QR 控制权重 / 介入区间 | 1.3 / 0.2–1.0 |
| Brightness 权重 / 介入区间 | 0.3 / 0.3–1.0 |

另一次低强度图生图记录使用 50 个配置步数、CFG 5.5、denoising strength 0.32、控制权重 1.3 / 0.25，两个控制区间均为 0–1；记录中的实际去噪步数为 16。它说明后续修整曾采用较小重绘范围，不证明该参数能修好任意二维码。

没有把这些本地参数对应的候选称为最终公主定稿。后续实际还使用了图像编辑工具、局部明暗修复、装饰替换和确定性合成。

## 控制图与参数的解释

- 从本次原码可靠获得矩阵与功能区。不要复制历史任务的 25×25 尺寸、格距、原点或定位中心。
- 结构控制图与明暗控制图必须在相同坐标系中对齐。记录画布、码区、静区及从原码到控制图的变换。
- QR Code Monster v2 模型卡提供了每模块 16 像素与码外灰色背景的输入建议。它是条件图约定，不是让最终成品的静区随意变成灰色、被装饰占用的理由。适配其他后端时先读对应模型说明。
- 权重控制该条件的影响强度；介入起止值是去噪过程的比例，不是秒数。更换模型、采样器或图生图模式后，不假设同一数值仍有相同效果。
- 控制更强可能更明显地保留码结构，也可能影响画面；权重与可读性不是全局单调关系。比较实际候选，避免无限增大权重。
- 图生图 strength 与 ControlNet 的介入区间是不同参数。记录配置步数与实际执行的去噪步数。

## 构图与提示词

先明确主色、连续深色体块、浅色通道和定位造型。例如深色丝绸与棕色头发承担暗部，象牙色花瓣与珍珠承担亮部。细节保持参考画风一致，避免为修码把插画的手改成真人照片质感。

一个主题方向的提示词示例：

```text
Elegant illustrated boutique opening artwork, an adult princess with gentle
downcast eyes, flowing burgundy silk, ivory petals and smooth pearls,
delicate blush and champagne butterflies, coherent illustrated anatomy,
three floral medallions integrated into the composition, continuous flowing
forms, clear dark silk and light floral areas.
```

该示例只描述画面；没有绑定原码或执行控制，也没有证明可扫描。主题、人物、色彩、文字与负面提示应由本次用户请求决定。

## 资源与运行异常

选择本地后端时检查 GPU/MPS、可用内存、磁盘和预计任务量；Apple Silicon 使用统一内存，不将它误报为独立显卡显存。不要只因后端能加载权重就宣布出图成功。

历史 MPS 尝试遇到低精度数值异常与内存压力，曾使用 attention slicing、VAE slicing/tiling，以及将 VAE 以 FP32 放在 CPU 解码等办法。这些是针对当时运行时的处置，不是所有机器都应复制的默认配置。记录实际失败；出现黑图或 NaN 时先检查数值路径，再调整画面提示词。

模型、库和下载源见 [sources.md](sources.md)；本仓库不自动安装或下载它们。
