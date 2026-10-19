# 来源、Related Work 与使用关系

整理与链接核对日期：2026-10-19。区分模型下载来源、实际使用的方法、阅读参考和未复现的研究；不把外部方法的效果当成本 Skill 的成绩。

## 实际使用过的模型

1. **ReV Animated 1.2** — [本次历史下载源](https://huggingface.co/s6yx/ReV_Animated)。历史生成脚本加载 `rev_1.2.safetensors`；它是 SD 1.5 兼容的主模型选择，不是二维码结构控制模型。本仓库不分发该文件。
2. **QR Code Monster v2** — [Monster Labs 模型卡](https://huggingface.co/monster-labs/control_v1p_sd15_qrcode_monster)。历史中使用 v2 二维码条件权重；该模型卡说明条件输入与使用建议，同时明确生成码的可读性会变化。
3. **Brightness ControlNet** — [实际使用的 Diffusers 发布源](https://huggingface.co/latentcat/control_v1p_sd15_brightness)。该模型卡将模型归于 IoC Lab 并署名 @ciaochaos，用于明暗控制。下载发布账号与原模型贡献者分别归属，不将发布镜像冒称为本项目原创。

## 方法基础与教程

4. **Lvmin Zhang, Anyi Rao, Maneesh Agrawala. Adding Conditional Control to Text-to-Image Diffusion Models. 2023.** [论文](https://arxiv.org/abs/2302.05543)。ControlNet 条件控制的基础工作。
5. **Anthony Fu 等：AI QR Code 101.** [原文](https://antfu.me/posts/ai-qrcode-101)。历史中阅读的生成、参数介入区间、输入设计与扫描差异参考。本流程吸收其创作与识别之间需要迭代的经验，不直接复制文章示例图。
6. **用户提供的艺术二维码教程截图。** 是本次创作方向与双控制配方的启发之一。没有可核验原帖 URL 时，不编造链接，不转载截图，不将截图中的配方或作品冒认原创。历史中实际试过的配置见 [generation.md](generation.md)。

## Related Work 与参考工程

7. **Jia-Wei Liao et al. DiffQRCoder: Diffusion-Based Aesthetic QR Code Generation with Scanning Robustness Guided Iterative Refinement. WACV 2025, pp. 5916–5925.** [CVF 论文页](https://openaccess.thecvf.com/content/WACV2025/html/Liao_DiffQRCoder_Diffusion-Based_Aesthetic_QR_Code_Generation_with_Scanning_Robustness_Guided_WACV_2025_paper.html) · [作者项目页](https://jwliao1209.github.io/DiffQRCoder/)。涉及扫描鲁棒性引导及迭代优化；本仓库没有实现、集成或复现其 SRPG / SR-MPGD，不引用其扫描成功率作为本项目结果。
8. **Sergio Cordero / Qraft.** [参考工程](https://github.com/sergiocrol/qraft)。历史调研中查阅的 SD 与双 ControlNet 工程，包含服务化实现。此 Skill 不包含它的部署栈或修复算法。
9. **Cream-yj / Artistic QR Skill.** [独立兄弟仓库](https://github.com/Cream-yj/artistic-qr-skill)。先生成独立小物件，再按原矩阵确定性组装。它与本仓库的整图融合路线不同，两者可共用验码与保护已确认内容的经验；不是本 Skill 的安装依赖。

## 码区与验码工具

10. **DENSO WAVE.** [Point for determining the code area](https://www.qrcode.com/en/howto/code.html)。二维码码区与四模块静区要求。
11. **OpenCV.** [WeChat QRCode 官方模块](https://github.com/opencv/opencv_contrib/tree/4.x/modules/wechat_qrcode)。历史中使用的电脑端解码路径，不能等同于手机微信应用测试。
12. **ZXing-C++.** [项目仓库](https://github.com/zxing-cpp/zxing-cpp)。历史中的另一条解码路径；配置和预处理结果需要分别记录。
13. **Apple Vision / CoreImage.** [VNDetectBarcodesRequest](https://developer.apple.com/documentation/vision/vndetectbarcodesrequest) · [CIDetector](https://developer.apple.com/documentation/coreimage/cidetector)。历史中的 macOS 原生解码路径；缺少平台能力时应记未测试。

本仓库的 MIT 许可仅覆盖自身文档与原创内容，不替代上述模型、软件和第三方作品各自的许可。
