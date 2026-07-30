# OperitForge — 双生猪猪原生悬浮分身补丁

此补丁基于 Operit 上游提交 `98faa44c42da0cb08bcadd5fd53a29993172331b`，把现有悬浮聊天球替换为跨应用可见的小猪分身。

## 已实现

- 小猪显示在其他 Android 应用上方，复用 Operit 原有 `TYPE_APPLICATION_OVERLAY` 和前台服务。
- 单击：摸小猪，立即播放 4 帧本地反应。
- 拖动：移动悬浮小猪并保存位置。
- 双击或长按：展开 Operit 浮窗聊天。
- 连续摸 6 次：当前角色收到一次隐藏触发提示，根据角色卡主动说一句话。
- 主动触发提示设置为 `persistTurn=false`、`hideUserMessage=true`，不会把触发说明写进聊天记录。
- 20 分钟冷却，避免频繁调用模型。
- 回复完成后复用 Operit 原有结果气泡，3 秒后自动回到小猪。

## 应用补丁

```bash
git clone https://github.com/AAswordman/Operit.git
cd Operit
git checkout 98faa44c42da0cb08bcadd5fd53a29993172331b
python /path/to/OperitForge/apply_dual_pig_overlay.py .
./gradlew :app:assembleDebug
```

补丁脚本会为被替换的 Kotlin 文件生成 `.dual_pig.bak` 备份。

## 当前边界

- 这是原生源码补丁，不是单独 `.toolpkg`；必须重新编译并安装 Operit APK。
- 当前阈值固定为 6 次、冷却固定为 20 分钟，后续可接入设置页。
- 当前使用最近选中的浮窗聊天和角色卡。
- 尚未在真实 Android 设备完成 Gradle/运行测试；代码按上游当前接口编写，并包含结构锚点检查。
