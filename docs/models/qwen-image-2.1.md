# Qwen-Image 2.1 模型介绍与本地下载放置指引

万象绘镜的 Windows 算力端使用 ComfyUI 执行本地 Qwen-Image 2.1 生图任务。模型文件留在算力电脑，不随 NAS Docker 镜像分发；用户图片、提示词和模型均保留在本地环境。

## 当前项目使用的模型组合

当前 v02.06.4beta 的基础文生图和图片编辑使用 Qwen-Image 2.1 GGUF 主模型，以及配套的编码器、VAE 和 LoRA。下面是当前工作流的配套文件；不能只下载主模型，也不能靠修改文件名替代其他模型。

| 文件 | 约大小 | 模型根目录下的位置 | 用途 |
|---|---:|---|---|
| `qwen-image-2.1-UC-Q4_0.gguf` | 3.87 GiB | `unet/` | 主模型 |
| `qwen3vl_8b_w4a8_heretic.safetensors` | 5.88 GiB | `text_encoders/` | 编码器 |
| `qwen_image_2.1_vae_bf16.safetensors` | 0.63 GiB | `vae/` | VAE |
| `Qwen-Image-2.1-viggle-turbo-4step-lora-r64.safetensors` | 0.32 GiB | `loras/` | 文生图 LoRA |
| `elusarcas-qwen2-1-detailer-v1.safetensors` | 0.07 GiB | `loras/` | 文生图细节 LoRA |
| `Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r128.safetensors` | 0.63 GiB | `loras/` | 图片编辑 LoRA |

六个文件合计约 11.40 GiB，大小来自本机文件统计，不是运行时显存需求，也不是 v01 所有功能的模型资源总量。已有兼容变体可以保留，实际可用性以工作流、节点和硬件为准。

## 下载后放置位置

程序和模型分开存放，推荐保持同级目录：

```text
任意目录/
  Omnivista/
    Omnivista Compute Launcher.exe
    runtime/
    workflows/
  Omnivista_models/          ← 启动器“模型目录”选择这里
    unet/                   ← 主模型
    text_encoders/          ← 编码器
    vae/                    ← VAE
    loras/                  ← 配套 LoRA
```

也可以使用其他磁盘上的已有模型目录，只需目录下包含相应分类。启动器选择这些分类文件夹的共同根目录，不选择某一个权重文件或 `unet` 子目录。

现有启动流程会将所选根目录映射给 ComfyUI，无需修改 NAS 地址或 ComfyUI 服务地址。修改模型根目录后，先停止当前算力，再从 NAS Web 重新启动算力，使后端读取新路径。

不要把模型放入 NAS Docker 数据卷，也不要上传到本公开下载仓库。完整运行环境包不包含权重，模型需自行准备。

## 在万象绘镜中核对

1. 打开 Windows 启动器，选择运行环境与模型根目录，填写 NAS 地址并连接；首次接入由 Web 管理员确认。
2. 在 NAS Web 算力中心确认电脑在线，查看上报的硬件和模型，保存对应电脑的模型配置。
3. 从 Web 启动算力；本机启动器不需要额外手动打开 ComfyUI。
4. 自行提交一张小尺寸测试图。若模型加载失败，检查文件名、目录、显存和 ComfyUI 节点是否匹配。环境检查通过不等于实际生图已验收。

## 下载来源

模型仓库：

<https://huggingface.co/KasugaSakura/Qwen-Image-2.1-Uncensored-Abzenrps-GGUF>

相关文本编码器仓库：

<https://huggingface.co/potkato/Qwen-Image-2.1-Text-Encoder-Heretic>

从对应仓库取得的主模型和编码器分别放入 `Omnivista_models/unet/`、`Omnivista_models/text_encoders/`，并与工作流和节点版本保持匹配。

以上是既有第三方主模型与编码器下载入口，不表示两个仓库都提供表中全部文件。VAE 和三份 LoRA 同样需要按表中完整文件名准备，不能遗漏；未确认的单文件下载地址不在此编造。

下载后按上面的目录说明放置，并在 ComfyUI 中刷新模型列表。仓库中的文件是否适合当前显卡、节点版本和工作流，需要在本机测试确认。

## 使用免责声明

- 本项目只提供模型文件名、目录结构和配置指引，不分发模型文件，也不替用户下载模型。
- 模型由第三方仓库维护，万象绘镜项目不拥有该模型仓库，也不保证仓库内容持续可用、完整、无错误或适用于所有硬件。
- 使用者应自行阅读模型仓库的许可证、使用限制、内容政策和当地法律法规，并自行承担下载、安装、运行和生成内容的责任。
- 模型生成结果可能包含错误、不准确或不适当内容；不得将生成结果用于违法、侵权、欺诈、伤害他人或绕过平台规则的用途。
- 下载前请自行确认来源可信并做好安全检查；因第三方模型、节点、下载地址或本机环境造成的损失，不由本项目承担。
