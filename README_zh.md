# MMD Studio — 测试版 v1.0

面向 MMD（MikuMikuDance）的 iOS PMX 几何预览工具基础版本。

## v1.0 测试版包含的功能

- SwiftUI iOS 应用外壳（iOS 17 及以上）
- PMX 2.0 / 2.1 文件头解析
- 顶点与三角面索引解析
- 支持跳过 BDEF1、BDEF2、BDEF4、SDEF、QDEF 五类顶点权重记录
- 纹理路径与材质元数据解析
- 基于 Metal 的索引三角面渲染
- 拖动旋转视角、双指捏合或按钮缩放
- GitHub Actions 工作流，可构建未签名 IPA 产物

## 本测试版未包含的功能

- 纹理图片加载与 PMX 材质着色
- 骨骼层级与 GPU 蒙皮
- 表情（Morph）动画
- VMD / VPD 导入与播放
- IK 与 MMD 物理
- 相机与灯光编辑器
- 离线高质量静帧渲染与 MOV 导出
- PMX 回写导出

这是一个**仅用于几何预览的首版测试构建**，还不是功能完整的 MMD 渲染器。解析器目前读到材质段就停止，尚未解析 PMX 后续的骨骼、表情、显示枠、刚体与关节等段落。

## 使用 GitHub Actions 构建

1. 新建一个 GitHub 仓库，把本文件夹的内容上传上去。
2. 推送到 `main` 或 `master` 分支，或者手动运行 `iOS Build` 工作流。
3. 在构建记录中下载名为 `MMDStudio-v1.0-test` 的产物（含未签名 IPA 与 build.log）。

该 IPA 未签名。要安装到实体 iPhone，需要苹果代码签名与描述文件（provisioning profile）。只有在 GitHub 的 macOS 运行器上成功完成构建后，才能把该产物视为已验证。

## 本地构建

需要 macOS、Xcode 16 及以上版本，以及 XcodeGen：

```bash
brew install xcodegen
xcodegen generate
xcodebuild -project MMDStudio.xcodeproj \
  -scheme MMDStudio \
  -sdk iphoneos \
  -configuration Release \
  -derivedDataPath build \
  CODE_SIGNING_ALLOWED=NO
```

## 首次真机测试

请使用一个体积较小、确认可用的 PMX 2.0/2.1 模型。把它的纹理文件与 PMX 放在同一目录，便于后续实现纹理支持；当前 v1.0 只用单色材质做诊断渲染。建议依次检查：

- PMX 文件选择器与解析状态是否正常
- 模型名称与几何数量是否正确
- 视口内是否能看到模型
- 拖动旋转与缩放按钮是否生效
- 导入大型模型时的内存表现

## 工程结构

- `App/`：SwiftUI 应用与导入流程
- `Core/`：PMX 数据结构与二进制解析器
- `Renderer/`：Metal 预览渲染器与着色器
- `.github/workflows/`：未签名 IPA 构建

## 中文化说明

本目录是原项目的简体中文版：

- 界面文案、按钮提示、无障碍标签、状态栏与解析错误信息均已改为简体中文
- 模型名称仍读取 PMX 文件内的原始名称（`nameJP` 优先），不做翻译
- 代码标识符、日志级别、Metal 着色器函数名保持原样，避免影响构建

英文说明见 [README.md](README.md)。
